## 하이브리드 클라우드 인프라 통합 명세서 (Markdown 최적화 레이아웃)

마크다운(Markdown) 환경에서 가로 폭을 최대로 활용하면서도 각 항목의 구분이 명확하도록 레이아웃을 전면 수정했습니다. **통합 명칭**과 **상세 용도**를 컬럼으로 분리하고, 스토리지 IP가 있는 경우에만 수직으로 병기하여 줄바꿈을 최소화했습니다.

---

### 1. 네트워크 계층 및 트래픽 분리 설계

본 인프라는 트래픽 성격에 따라 두 개의 가상 네트워크(VLAN)를 운영하여 성능과 보안을 동시에 확보합니다.

* **VLAN 10/20 (Service/Mgmt):** 서버 관리 및 대외 서비스 접속 전용 (`192.168.10.x`)
* **VLAN 30 (Storage/DB Sync):** Ceph 데이터 복제 및 DB 실시간 동기화 전용 (`192.168.30.x`)

---

### 2. 통합 인프라 상세 구성 목록

| 호스트 | VM 번호 | IP 주소 (서비스 / 스토리지) | 통합 명칭 | 상세 용도 및 기술 스택 |
| --- | --- | --- | --- | --- |
| **SRV-01** | VM-01 | **192.168.10.11** | **Cloud Master** | OpenStack 제어부 및 인프라 통합 DNS (Bind9) |
| **SRV-02** | VM-02 | **192.168.10.12** | **Hybrid Gateway** | AWS/GCP VPN 터널링 엔드포인트 및 IaC 제어 센터 |
| **SRV-03** | VM-03 | **192.168.10.13**<br>**192.168.30.13** | **Storage Node 01** | Ceph OSD 분산 노드 및 NFS 공유 파일 서버 |
| **SRV-04** | VM-04 | **192.168.10.14**<br>**192.168.30.14** | **Storage Node 02** | Ceph OSD 및 S3 API 호환 Object Storage Gateway |
| **SRV-05** | VM-05 | **192.168.10.15**<br>**192.168.30.15** | **DB Master** | PostgreSQL & MySQL 통합 데이터 소스 마스터 노드 |
| **SRV-06** | VM-06 | **192.168.10.16**<br>**192.168.30.16** | **DB Slave** | 실시간 데이터 복제(Replication) 및 재해 복구 노드 |
| **SRV-07** | VM-07 | **192.168.10.17** | **Django App 01** | Nginx + Gunicorn 기반 Python 웹 서비스 메인 노드 |
| **SRV-08** | VM-08 | **192.168.10.18** | **Django App 02** | 고가용성 보장을 위한 L4/L7 로드밸런싱 실습 노드 |
| **SRV-09** | VM-09 | **192.168.10.19** | **K8s Master** | Kubernetes 클러스터 관리 및 오케스트레이션 제어부 |
| **SRV-10** | VM-10 | **192.168.10.20**<br>

<br>**192.168.30.20** | **K8s Worker** | Pod 실행 환경 및 Ceph CSI 기반 볼륨 연동 노드 |
| **SRV-11** | VM-11 | **192.168.10.21** | **DevOps Center** | Jenkins CI/CD 파이프라인 및 Python venv 환경 통합 |
| **SRV-12** | VM-12 | **192.168.10.22** | **AI Serving** | Django/FastAPI 기반 인공지능 모델 추론 API 서버 |
| **SRV-13** | VM-13 | **192.168.10.23** | **Task Worker** | Redis 메시지 브로커 및 Celery 비동기 작업 처리 |
| **SRV-14** | VM-14 | **192.168.10.24** | **Monitor Hub** | Prometheus, Grafana, ELK 기반 통합 인프라 관제 |
| **SRV-15** | VM-15 | **192.168.10.25** | **Security Pool** | HashiCorp Vault 비밀 관리 및 하이브리드 확장 풀 |

---

### 3. 설계 핵심 포인트

* **가로 가독성 최적화:** IP 주소 컬럼에 `<br>` 태그를 사용하여 수직으로 배치함으로써 호스트명과 통합 명칭 컬럼이 줄바꿈 없이 넓게 유지되도록 조정했습니다.
* **VLAN 30 트래픽 격리:** 데이터 집약적 통신이 필요한 노드에만 전용 인터페이스를 할당하여 서비스 품질(QoS)을 보장합니다.
* **현대적 스택 통합:** Django 기반의 웹 서비스와 AI 추론, 비동기 작업 큐의 유기적 연동을 통해 실무와 동일한 하이브리드 환경을 구축합니다.

[출처: OpenStack Networking Advanced Design](https://docs.openstack.org/neutron/latest/)

[출처: Django Production Deployment Best Practices](https://docs.djangoproject.com/en/stable/howto/deployment/)

---

Next: VLAN 30 기반의 PostgreSQL 스트리밍 복제 및 Django DB 라우팅 설정

VLAN 30 격리, Django 통합 설계, Ceph OSD 복제, 하이브리드 게이트웨이.
