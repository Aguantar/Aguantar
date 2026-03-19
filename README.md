<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,100:1a1b2e&height=200&section=header&text=이준서&fontSize=42&fontColor=ffffff&fontAlignY=35&desc=Data%20Engineer%20%7C%20Real-time%20Pipeline%20%7C%20On-Premise&descSize=16&descColor=8b949e&descAlignY=55" width="100%"/>

<br/>

미니 PC 온프레미스 환경에서 **실시간 데이터 파이프라인을 직접 구축하고 24/7 운영**합니다.
<br/>
운영 경험을 **Claude Code MCP 서버와 Skills로 코드화**하여 오픈소스로 배포합니다.

<br/><br/>

[![Blog](https://img.shields.io/badge/Blog-calme.tistory.com-ffffff?style=for-the-badge&logo=tistory&logoColor=white&labelColor=0d1117)](https://calme.tistory.com)
&nbsp;
[![PyPI](https://img.shields.io/badge/PyPI-Aguantar-3775A9?style=for-the-badge&logo=pypi&logoColor=white&labelColor=0d1117)](https://pypi.org/user/Aguantar/)

</div>

<br/>

## 🎮 Circuit Connect — "불을 켜줘!"

<table>
<tr>
<td width="55%">

### 회로를 연결해서 불을 켜는 퍼즐 게임

**원스토어 + 앱인토스(Toss 미니앱)** 에 배포한 모바일 게임입니다.

플레이어가 회로 조각을 돌려서 전구에 불을 켜는 퍼즐이고,
게임 이벤트는 실시간으로 **Kafka → Flink → ClickHouse** 파이프라인을 통해 수집·분석됩니다.

<br/>

| Metric | Value |
|:-------|:------|
| 유저 | **74명** |
| 배포 | **원스토어 · 앱인토스** |
| 이벤트 | **8,475건 (18일)** |
| 모드 | 스토리 · 타임어택 |
| A/B 테스트 | cityHash64 기반 |

<br/>

[![Repo](https://img.shields.io/badge/Frontend-circuit--connect-61DAFB?style=flat-square&logo=react&logoColor=white)](https://github.com/Aguantar/circuit-connect)

</td>
<td width="45%">

```
React / TypeScript (Frontend)
FastAPI (Backend)
Kafka → Flink → ClickHouse (Pipeline)
Grafana (Analytics Dashboard)

Session Aggregation (Flink)
Anomaly Detection (Z-score)
A/B Testing (cityHash64)
```

**게임 파이프라인 구조**
```
📱 클라이언트
 ↓ game-events
⚡ Kafka (4 partitions)
 ↓
🔄 Flink (세션 집계 · 이상 탐지)
 ↓
🗄️ ClickHouse (fact_sessions · game_alerts)
 ↓
📊 Grafana (실시간 대시보드)
```

</td>
</tr>
</table>

<br/>

---

<br/>

## ⚡ CDC Realtime Pipeline — 실시간 암호화폐 데이터 파이프라인

<table>
<tr>
<td width="55%">

Upbit 거래소에서 **5종 코인(BTC/ETH/SOL/XRP/DOGE)** 의 체결 데이터를
실시간으로 수집하고, 이상 거래를 탐지하는 CDC 파이프라인입니다.

<br/>

| Metric | Value |
|:-------|:------|
| 처리량 | **~680K trades/day** |
| 누적 데이터 | **20M+ 거래** |
| E2E 레이턴시 | **p50 1.8s · p99 3.4s** |
| CDC 레이턴시 | **p50 3ms** |
| 이상 탐지 | **EWMA 기반, 96% FP 감소 (v1→v3)** |
| 인프라 | **Docker 28컨테이너 · 24/7** |

<br/>

[![Repo](https://img.shields.io/badge/Repo-cdc--realtime--pipeline-E6526F?style=flat-square&logo=apacheflink&logoColor=white)](https://github.com/Aguantar/cdc-realtime-pipeline)

</td>
<td width="45%">

```
Debezium (MySQL CDC)
 ↓
Kafka (3-broker, RF=3)
 ↓
Flink (변환 · 이상 탐지)
 ↓
ClickHouse (OLAP)
 ↓
Grafana · dbt · Airflow · n8n
```

**50시간 Flink 장애 경험**
```
MySQL DELETE → Debezium tombstone
→ Flink NPE → Job FAILED
→ 50시간 무인 장애

교훈:
- NullSafeStringSchema 구현
- fixedDelayRestart(3, 10s)
- 복합 조건 모니터링 필요
→ 이 경험이 MCP/Skills 개발 동기
```

</td>
</tr>
</table>

<br/>

---

<br/>

## 🔧 DataOps Agent Skills — MCP 서버 & Skills 개발

<table>
<tr>
<td width="55%">

CDC 파이프라인 운영 중 겪은 **장애 대응 · 진단 노하우**를
Claude Code **MCP 서버(도구)** 와 **Skills(매뉴얼)** 로 코드화한 프로젝트입니다.

MCP는 Claude가 ClickHouse/Kafka에 직접 접근하는 도구이고,
Skills는 그 데이터를 어떻게 해석할지 알려주는 진단 트리입니다.

<br/>

| 산출물 | 상세 |
|:-------|:-----|
| **MCP 서버** | ClickHouse (8도구) · Kafka (4도구) · VibeScan (2도구) |
| **Skills** | kafka-debug · flink-ops · clickhouse-ops · dbt-workflow · game-analytics · incident-report |
| **Commands** | /pipeline · /data-quality · /incident · /game-ops |
| **배포** | GitHub · PyPI · MCP Registry · Plugin Marketplace |

</td>
<td width="45%">

**설치**
```bash
pip install clickhouse-dataops-mcp
pip install kafka-dataops-mcp
```

**사용**
```
"파이프라인 상태 확인해줘"

→ MCP 4도구 병렬 호출
→ Skills 진단 트리로 해석
→ 구조화된 리포트 자동 생성
```

**핵심 진단 로직**
```
Flink group
+ no active members
+ lag ≥ 1,000
→ Flink 크래시 의심
  (50시간 장애 패턴)
```

</td>
</tr>
</table>

<div align="center">

[![CH MCP](https://img.shields.io/badge/ClickHouse_MCP-8_tools-818cf8?style=flat-square)](https://github.com/Aguantar/clickhouse-mcp-server)
&nbsp;
[![Kafka MCP](https://img.shields.io/badge/Kafka_MCP-4_tools-3b82f6?style=flat-square)](https://github.com/Aguantar/kafka-mcp-server)
&nbsp;
[![Skills](https://img.shields.io/badge/Skills-6_skills_·_4_commands-10b981?style=flat-square)](https://github.com/Aguantar/dataops-skills)
&nbsp;
[![VibeScan MCP](https://img.shields.io/badge/VibeScan_MCP-2_tools-a855f7?style=flat-square)](https://github.com/Aguantar/vibescan-mcp-server)

</div>

<br/>

---

<br/>

## 🛡️ VibeScan — 바이브코더를 위한 코드 보안 스캐너

<table>
<tr>
<td>

바이브코딩으로 만든 프로젝트를 배포 전에 점검하는 **CLI 보안 스캐너**입니다.
코드는 외부로 나가지 않고 **완전 로컬에서 실행**됩니다.

**17개 탐지 규칙 · 14개 시크릿 카테고리 · 196개 테스트 케이스**

```bash
pip install vibescan
vibescan scan ./my-project
```

</td>
<td width="35%">

[![Repo](https://img.shields.io/badge/CLI-vibescan-10b981?style=flat-square&logo=python&logoColor=white)](https://github.com/Aguantar/vibescan)
[![MCP](https://img.shields.io/badge/MCP-vibescan--mcp-a855f7?style=flat-square)](https://github.com/Aguantar/vibescan-mcp-server)
[![PyPI](https://img.shields.io/badge/PyPI-vibescan-3775A9?style=flat-square&logo=pypi&logoColor=white)](https://pypi.org/project/vibescan/)

</td>
</tr>
</table>

<br/>

---

<br/>

## 📊 기타 프로젝트

| Project | Description | Highlight |
|:--------|:------------|:----------|
| [**fds-pipeline-lab**](https://github.com/Aguantar/fds-pipeline-lab) | 이상거래 탐지 파이프라인 | TPS 70 → 17,500 성능 개선 |
| [**hotel**](https://github.com/Aguantar/hotel) | 호텔 예약 플랫폼 | Vue 3 + Spring Boot 3 |
| [**seoul-auction-prediction**](https://github.com/Aguantar/seoul-auction-prediction) | 서울 부동산 경매가 예측 | 16,368건, MAE 49.1% 개선 |

<br/>

---

<br/>

## Tech Stack

<div align="center">

**Data Engineering** &nbsp;&nbsp;
![Kafka](https://img.shields.io/badge/Kafka-231F20?style=flat-square&logo=apachekafka&logoColor=white)
![Flink](https://img.shields.io/badge/Flink-E6526F?style=flat-square&logo=apacheflink&logoColor=white)
![ClickHouse](https://img.shields.io/badge/ClickHouse-FFCC01?style=flat-square&logo=clickhouse&logoColor=black)
![dbt](https://img.shields.io/badge/dbt-FF694B?style=flat-square&logo=dbt&logoColor=white)
![Spark](https://img.shields.io/badge/Spark-E25A1C?style=flat-square&logo=apachespark&logoColor=white)

**Languages** &nbsp;&nbsp;
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Java](https://img.shields.io/badge/Java-007396?style=flat-square&logo=openjdk&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat-square&logo=mysql&logoColor=white)

**Infrastructure** &nbsp;&nbsp;
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Airflow](https://img.shields.io/badge/Airflow-017CEE?style=flat-square&logo=apacheairflow&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-F46800?style=flat-square&logo=grafana&logoColor=white)
![Tableau](https://img.shields.io/badge/Tableau-E97627?style=flat-square&logo=tableau&logoColor=white)

**Database** &nbsp;&nbsp;
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![BigQuery](https://img.shields.io/badge/BigQuery-4285F4?style=flat-square&logo=googlebigquery&logoColor=white)

</div>

<br/>

---

<br/>

<div align="center">

<img height="165" src="https://github-readme-stats.vercel.app/api?username=Aguantar&show_icons=true&theme=github_dark&hide_border=true&icon_color=2F81F7&title_color=2F81F7&bg_color=0d1117" />
<img height="165" src="https://github-readme-stats.vercel.app/api/top-langs/?username=Aguantar&layout=compact&theme=github_dark&hide_border=true&title_color=2F81F7&bg_color=0d1117" />

<br/>

[![GitHub Streak](https://streak-stats.demolab.com?user=Aguantar&theme=github-dark-blue&hide_border=true&date_format=Y.n.j)](https://git.io/streak-stats)

</div>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,100:1a1b2e&height=100&section=footer" width="100%"/>
