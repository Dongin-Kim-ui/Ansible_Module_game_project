## 🎮 Ansible 기반 Kubernetes 게임 배포 자동화

이 프로젝트는 Ansible을 활용하여 게임 애플리케이션을 Kubernetes 클러스터에 자동 배포하고, HAProxy와 WAF 설정까지 포함한 전체 인프라를 자동화합니다.

---

### 📁 디렉터리 구조
```
game_ansible/
├── group_vars/
│   └── all.yml
├── host_vars/
│   ├── master.yml
│   └── worker.yml
├── inventory/
│   └── hosts.ini
├── roles/
│   ├── k8s_deploy/
│   │   ├── tasks/main.yml
│   │   └── templates/*.j2
│   ├── haproxy/
│   │   ├── tasks/main.yml
│   │   └── templates/haproxy.cfg.j2
│   ├── nas/
│   │   └── tasks/main.yml
│   └── waf/
│       └── tasks/main.yml
├── playbook.yml
└── README.md
```

---

### ⚙️ 주요 기능
- Kubernetes에 Deployment/Service/Ingress 자동 생성
- 로그 수집 및 게임 데이터 NAS 백업
- HAProxy 구성 및 포트 오픈
- WAF에서 DNAT/MASQUERADE 방화벽 자동 설정

---

### 🛠 실행 방법
```bash
ansible-playbook -i inventory/hosts.ini playbook.yml
```

> 사전에 `group_vars/all.yml`에 배포 대상 정보를 설정해야 합니다.

---

### 📋 변수 예시 (`group_vars/all.yml`)
```yaml
game_name: "minipunk"
app_name: "minipunk-app"
deployment_name: "minipunk-deploy"
service_name: "minipunk-svc"
container_image: "pangkin/minipunk"
container_port: "5000"
node_port: "30002"
nas_server: "192.168.20.100"
backup_storage_path: "/mnt/game-pool/gamedata/{{ game_name }}/backup"
log_storage_path: "/mnt/game-pool/gamelogs/{{ game_name }}/logs"
haproxy_config_path: "/etc/haproxy/haproxy.cfg"
```

---

### 📦 템플릿 경로
- `deployment.yaml.j2`, `service.yaml.j2`, `ingress.yaml.j2`
- `haproxy.cfg.j2`

템플릿은 Ansible의 `template` 모듈을 통해 Kubernetes YAML 또는 설정 파일로 변환됩니다.

---

### 🙋‍♂️ 문의 및 확장
- 게임 수 추가 시 `group_vars/all.yml`만 수정하여 반복 배포 가능
- 다양한 게임 이미지 및 포트를 지원하며 HA 구성을 유지합니다
