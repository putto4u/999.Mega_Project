마크다운 표 자체에는 개별 행 사이에 굵은 가로줄을 넣는 표준 문법이 없지만, **호스트 서버별로 행을 병합하거나 구분선 역할을 하는 빈 행**을 삽입하여 시각적으로 분리할 수 있습니다.

여기서는 각 호스트 서버(SRV-XX) 단위로 데이터가 묶여 보일 수 있도록 레이아웃을 최적화한 최종 명세서입니다.

---

## 하이브리드 클라우드 인프라 통합 설계 명세서 (호스트별 가로 구분 뷰)

| 호스트 서버 | VM 번호 | IP 주소 (서비스 / 스토리지) | 통합 명칭 및 상세 용도 |
| --- | --- | --- | --- |
| **SRV-01** | VM-01 | **192.168.10.11** | **Cloud Master Controller** <br> OpenStack 제어부 및 DNS (Bind9) |
| **SRV-02** | VM-02 | **192.168.10.12** | **Hybrid Gateway**<br>

<br>VPN 터널링 및 IaC 제어 센터 |
| **SRV-03** | VM-03 | **192.168.10.13**<br>

<br>**192.168.30.13** | **Distributed Storage-01**<br>

<br>Ceph OSD 및 NFS 공유 파일 서버 |
| **SRV-04** | VM-04 | **192.168.10.14**<br>

<br>**192.168.30.14** | **Distributed Storage-02**<br>

<br>Ceph OSD 및 S3 Object Gateway |
| **SRV-05** | VM-05 | **192.168.10.15**<br>

<br>**192.168.30.15** | **Integrated DB Master**<br>

<br>PostgreSQL & MySQL 통합 마스터 |
| **SRV-06** | VM-06 | **192.168.10.16**<br>

<br>**192.168.30.16** | **Standby DB Slave**<br>

<br>실시간 DB 복제 및 재해 복구 노드 |
| **SRV-07** | VM-07 | **192.168.10.17** | **Django Web/App-01**<br>

<br>Nginx + Gunicorn + Django 메인 |
| **SRV-08** | VM-08 | **192.168.10.18** | **Django Web/App-02**<br>

<br>L4/L7 로드밸런싱 실습 노드 |
| **SRV-09** | VM-09 | **192.168.10.19** | **K8s Control Plane**<br>

<br>Kubernetes 클러스터 관리 제어부 |
| **SRV-10** | VM-10 | **192.168.10.20**<br>

<br>**192.168.30.20** | **K8s Worker Node**<br>

<br>Pod 실행 및 Ceph CSI 스토리지 연동 |
| **SRV-11** | VM-11 | **192.168.10.21** | **DevOps Center**<br>

<br>Jenkins CI/CD 및 개발 환경 통합 |
| **SRV-12** | VM-12 | **192.168.10.22** | **AI Model Serving**<br>

<br>Django/FastAPI AI 모델 API 서빙 |
| **SRV-13** | VM-13 | **192.168.10.23** | **Asynchronous Task**<br>

<br>Redis + Celery 비동기 작업 처리 |
| **SRV-14** | VM-14 | **192.168.10.24** | **Full-Stack Monitor**<br>

<br>Prometheus, Grafana 통합 관제 |
| **SRV-15** | VM-15 | **192.168.10.25** | **Security & Scale Pool**<br>

<br>Vault 비밀 관리 및 클라우드 확장 풀 |

---

### 기술적 설계 반영 사항 및 팁

* **가로줄 구분:** 마크다운 표준에서는 셀마다 가로줄을 넣는 기능이 제한적이므로, 각 서버명(SRV-XX)을 **굵게(Bold)** 처리하여 시각적인 단락 구분을 명확히 했습니다.
* **VLAN 30 활용:** 데이터 복제 트래픽이 서비스 대역에 영향을 주지 않도록 물리적/논리적으로 격리된 경로를 보장합니다.
* **현대적 스택 통합:** Django와 연동되는 AI 서빙 및 비동기 큐를 통해 엔드 투 엔드 서비스 구현이 가능합니다.

[출처: OpenStack Networking Advanced Design](https://docs.openstack.org/neutron/latest/)

[출처: Django Production Deployment Best Practices](https://docs.djangoproject.com/en/stable/howto/deployment/)

---

Next: VLAN 30 기반의 PostgreSQL 스트리밍 복제 및 Django DB 라우팅 설정

VLAN 30 격리, Django 통합 설계, Ceph OSD 복제, 하이브리드 게이트웨이.
