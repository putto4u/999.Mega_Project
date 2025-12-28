## 하이브리드 클라우드 인프라 통합 명세서 (PC 20대 풀 구성안)

강의 실습을 위해 기존에 정의했던 **15대의 핵심 인프라 VM**에 더하여, 실습의 완성도를 높이고 대규모 클러스터링 및 사용자 시뮬레이션을 위한 **나머지 5대(VM-16 ~ VM-20)를 포함한 전체 20대**의 최종 인프라 목록입니다.

---

### 1. 전체 인프라 네트워크 레이아웃

모든 자원은 서비스 및 관리용 **VLAN 10/20**과 데이터 전송 전용 **VLAN 30**으로 구분됩니다.

---

### 2. 통합 인프라 상세 구성 목록 (VM-01 ~ VM-20)

| 호스트 | VM 번호 | IP 주소 (서비스 / 스토리지) | 통합 명칭 | 상세 용도 및 기술 스택 |
| --- | --- | --- | --- | --- |
| **SRV-01** | VM-01 | **192.168.10.11** | **Cloud Master** | OpenStack 제어부 및 인프라 통합 DNS (Bind9) |
| **SRV-02** | VM-02 | **192.168.10.12** | **Hybrid Gateway** | AWS/GCP VPN 터널링 및 IaC 제어 센터 (Terraform) |
| **SRV-03** | VM-03 | **192.168.10.13**<br>

<br>**192.168.30.13** | **Storage Node 01** | Ceph OSD 분산 노드 및 NFS 공유 파일 서버 |
| **SRV-04** | VM-04 | **192.168.10.14**<br>

<br>**192.168.30.14** | **Storage Node 02** | Ceph OSD 및 S3 API 호환 Object Storage Gateway |
| **SRV-05** | VM-05 | **192.168.10.15**<br>

<br>**192.168.30.15** | **DB Master** | PostgreSQL & MySQL 통합 데이터 소스 마스터 노드 |
| **SRV-06** | VM-06 | **192.168.10.16**<br>

<br>**192.168.30.16** | **DB Slave** | 실시간 데이터 복제(Replication) 및 재해 복구 노드 |
| **SRV-07** | VM-07 | **192.168.10.17** | **Django App 01** | Nginx + Gunicorn 기반 Python 웹 서비스 메인 노드 |
| **SRV-08** | VM-08 | **192.168.10.18** | **Django App 02** | 고가용성 보장을 위한 L4/L7 로드밸런싱 실습 노드 |
| **SRV-09** | VM-09 | **192.168.10.19** | **K8s Master** | Kubernetes 클러스터 관리 및 오케스트레이션 제어부 |
| **SRV-10** | VM-10 | **192.168.10.20**<br>

<br>**192.168.30.20** | **K8s Worker 01** | Pod 실행 환경 및 Ceph CSI 기반 볼륨 연동 노드 |
| **SRV-11** | VM-11 | **192.168.10.21** | **DevOps Center** | Jenkins CI/CD 파이프라인 및 Python venv 환경 통합 |
| **SRV-12** | VM-12 | **192.168.10.22** | **AI Serving** | Django/FastAPI 기반 인공지능 모델 추론 API 서버 |
| **SRV-13** | VM-13 | **192.168.10.23** | **Task Worker** | Redis 메시지 브로커 및 Celery 비동기 작업 처리 |
| **SRV-14** | VM-14 | **192.168.10.24** | **Monitor Hub** | Prometheus, Grafana, ELK 기반 통합 인프라 관제 |
| **SRV-15** | VM-15 | **192.168.10.25** | **Security Center** | HashiCorp Vault 비밀 관리 및 보안 감사 로그 수집 |
| **SRV-16** | VM-16 | **192.168.10.26** | **K8s Worker 02** | 대규모 컨테이너 배포를 위한 추가 워커 노드 |
| **SRV-17** | VM-17 | **192.168.10.27**<br>

<br>**192.168.30.27** | **Storage Node 03** | Ceph 클러스터 데이터 가용성(Quorum) 확보 노드 |
| **SRV-18** | VM-18 | **192.168.10.28** | **Traffic Generator** | JMeter/Locust 기반 대량 트래픽 및 부하 테스트 노드 |
| **SRV-19** | VM-19 | **192.168.10.29** | **Bastion Host** | 외부 접속 단일 통제점 및 SSH 터널링 보안 서버 |
| **SRV-20** | VM-20 | **192.168.10.30** | **Backup Repository** | 인프라 전체 형상 데이터 및 DB 덤프 보관 전용 노드 |

---

### 3. 추가된 5대의 핵심 역할

* **VM-16 (K8s Worker 02):** 단일 워커 노드에서 발생할 수 있는 자원 부족을 방지하고, 쿠버네티스의 스케줄링 및 노드 장애 대응(Failover) 실습을 가능하게 합니다.
* **VM-17 (Storage 03):** Ceph 분산 파일 시스템의 홀수 구성을 완성하여 데이터 무결성을 보장하는 모니터링 노드 역할을 겸합니다.
* **VM-18 (Traffic Gen):** 대중 유저가 없는 실습 환경에서 Django 앱의 부하 처리 능력을 검증하기 위해 인위적인 트래픽을 생성합니다.
* **VM-19 (Bastion):** 모든 인프라 접근을 이 노드를 통해서만 가능하게 설정하여 실제 보안 아키텍처를 구현합니다.
* **VM-20 (Backup):** 실습 중 발생할 수 있는 데이터 손실에 대비하고, 백업 정책 수립 실습에 활용합니다.

[출처: Enterprise Infrastructure Scalability Guide](https://www.google.com/search?q=https://docs.openstack.org/operations-guide/ops-scaling.html)
[출처: Ceph Cluster Requirements](https://docs.ceph.com/en/latest/start/hardware-recommendations/)

---

Next: 20대 VM 통합 가동 및 VLAN 30 기반 스토리지 연결 테스트

VLAN 30 확장, K8s 노드 추가, 부하 테스트, 보안 베스천.
