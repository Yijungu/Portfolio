# 랜더링 및 라우팅 서버를 활용한 장애인 친화 네비게이션 서비스


# 목차

1. [📄 개요](#개요)
2. [📋 문제 정의 및 기획](#문제정의및기획)
2. [📐 디자인](#디자인)
4. [💻 개발 목표 및 사용 기술](#개발)
5. [💽 개발 내용](#개발내용)

## 개요

![썸네일](https://github.com/user-attachments/assets/f86aab9c-b4b9-41f7-bc77-841d55bc949e)


> ‘**모두의 지도**’는 충남대학교 내 장애학생 및 교통 약자를 위한 **배리어 프리 캠퍼스 안내 서비스**입니다. 장애인 전용 화장실, 경사로, 전동 휠체어 충전소 등 편의시설 위치 및 이동 경로를 시각화하여 안내합니다.

## 문제정의
- 현재 교내 장애인들은 장애인 편의시설의 설치 유무와 위치 파악 어려움
- 이동성 제약으로 적절한 경로를 찾지 못할 경우 더 큰 어려움 발생

## 기확 및 디자인
### 기획
| 📍 문제 정의 |  | 🎯 핵심 기능 기획 |
| --- | --- | --- |
| 이동성 제약으로 적절한 경로를 찾지 못할 경우 더 큰 어려움 발생 | ➡️ | 휠체어 이용자들을 위한 적절한 경로 안내를 제공 |
| 교내 편의시설 (ex. 장애인 화장실, 진입 경사로, 승강기) 의 설치 유무와 승강기 위치 파악에 대한 어려움 |➡️ | 편의시설 안내 및 건물 내의 상세 위치 정보 제공 |
| 휠체어 이용자의 제한된 손 사용성 | ➡️ | 한손 조작 가능한 UI/UX 제공|

### 세부 기획
#### 휠체어 이용자들을 위한 적절한 경로 안내를 제공
>휠체어 이용자들의 불편함을 파악하고자 인터뷰 및 정보 탐색
1. 경사로를 피한 경사로 안내
2. 계단을 피한 경사로 안내
3. 골목길을 피한 경사로 안내

#### 편의시설 안내 및 건물 내의 상세 위치 정보 제공
>충남대학교 시설과에 연락하여 학교의 도면을 받아 장애인 편의 시설에 대한 정리
1. 건물 내에 편의시설(ex. 장애인 화장실) 정보 제공
3. 교내 장애인 시설(ex. 장애인 휴게실) 정보 제공
2. 정확한 위치 파악을 위한 건물 안내도 제공

#### 한손 조작 가능한 UI/UX
>타자를 치지 않아도 가능한 모든 핵심 기능들을 사용할 수 있게끔 설계
1. 태그 기반 편의 시설 검색
2. 내 위치 기반, 클릭 기반하여 목적지 입력 없이 길찾기 실행
3. 건물 클릭하여 장애 편의 시설 상세 정보 
4. 정확한 위치 파악을 위한 건물 안내도 제공

### 디자인
#### 한손 조작 가능한 UI/UX


## 개발 목표 및 기술
### 충남대 전역 지도 데이터 처리
> **OSM 데이터 전처리**
1. Google Elevation API를 활용한 충남대학교 전역의 지형 데이터 기반 고도 데이터 추출
2. 고도 데이터를 활용한 경로(way)의 경사도 계산 및 분석
3. PostGIS를 활용한 공간 데이터 구축 및 관리

### 휠체어 이용자들을 위한 적절한 경로 안내를 제공
> **Valhalla 라우팅 서버 구축**
1. 기존 오픈 소스 프로젝트를 분석하여 오픈소스를 커스터마이징 [Wheelchair Costing 알고리즘 ]
### 편의시설 안내 및 건물 내의 상세 위치 정보 제공

> **Tegola 랜더링 서버 구축**
1. 오픈소스 활용하여 json 파일을 설정하여 랜더링 가능한 벡터 파일 제공
2. 파일 기반 캐싱 전략으로 설정, 충남대학교의 타일을 미리 생성하여 저장

### 게이트웨이 및 데이터 관리
> **Rust 서버 구축**
1. Valhalla, Tegola, PostgreSQL (PostGIS)를 요청에 맞게 분배
2. 비동기 처리(tokio)를 활용하여 다수의 사용자 요청을 동시에 처리할 수 있도록 설계
3. 데이터 요청/응답을 통해 JSON 데이터를 처리하여 프론트엔드와 연동.

### 환경 일관성 유지 및 배포
> **Docker 환경 구축 및 AWS 배포**
1. Docker 내의 network를 활용하여 컨테이너 간 통신을 구성
2. Docker 이미지 경량화를 통해 AWS의 프리티어에서 메모리 부족 문제 해결 및 안정적인 배포

## 개발 내용

### 시스템 아키텍처

![stacks with flow (2)](https://github.com/user-attachments/assets/cbcc6a80-ff99-4081-b064-b4a0041c6098)



| FE | BE | Database | Others |
| --- | --- | --- | --- |
| <img src="https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=Flutter&logoColor=white"> <img src="https://img.shields.io/badge/mapbox-000000?style=for-the-badge&logo=mapbox&logoColor=white"> | <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=Docker&logoColor=white">  <img src="https://img.shields.io/badge/Rust-000000?style=for-the-badge&logo=Rust&logoColor=white"> | <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=PostgreSQL&logoColor=white">| <img src="https://img.shields.io/badge/Notion-000000?style=for-the-badge&logo=Notion&logoColor=white"> <img src="https://img.shields.io/badge/Discord-5865F2?style=for-the-badge&logo=Discord&logoColor=white"> |

### 개발 세부 내용

1. **활용 공공데이터**
    - **교내 데이터**    
        <table>
            <thead>
                <tr style="background-color: #f2f2f2;">
                    <th>건물 카테고리 데이터</th>
                    <th>장애인 편의시설 위치 데이터</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td><img src="https://github.com/user-attachments/assets/77e01425-a501-4ddf-a0b0-39576ed7a90e" alt="image" width="400"></td>
                    <td><img src="https://github.com/user-attachments/assets/24835c13-bef9-47ae-8674-59258c731606" alt="image" width="400"></td>
                </tr>
                <tr>
                    <td>교내 건물 카테고리 데이터를 통해 교내 건물 대분류, 소분류에 활용하였습니다.</td>
                    <td>휠체어 이용 장애인들에게 필요한 경사로, 장애인 화장실 정보 등을 교내 건물 도면을 이용해 직접 공공데이터화 하였습니다.</td>
                </tr>
            </tbody>
        </table>

    - **교외 데이터**
        | Open Street map |
        | --- |
        | <img src="https://github.com/user-attachments/assets/28726428-ee24-4de9-96c7-90123de61549" alt="OSM Image" width="200"> |
        | OSM 은 전 세계 커뮤니티가 협업하여 구축한 공개 지도 데이터베이스로 누구나 자유롭게 데이터를 수정, 추가, 활용할 수 있는 공공 데이터 플랫폼입니다. 본 서비스에선 직접 충남대학교 일부 건물에 한해 경사로, 장애인 화장실 정보를 추가한 뒤 MapBox API를 통해 렌더링하고 있습니다. 공개 지도 데이터베이스이므로 사용자들은 지도에 직접 위치를 추가할 수 있습니다. |
        | <img src="https://github.com/user-attachments/assets/10239f7e-7877-4f72-bed4-890d12042874" alt="image" width="400"> |


2. **ERD 설계서**
-  <img width="500" alt="erd" src="https://github.com/user-attachments/assets/196a8b31-78ed-4560-af8f-103f488f39cd" />


3. **프로젝트 구조**
    - **FE 프로젝트 구조**
      <details>
      <summary>FE 프로젝트 구조 보기</summary>
    
      ```plaintext
      📂 lib/
      ├── 📂 data/                      # 데이터 처리 관련 코드
      │   ├── 📂 local/                 # 로컬 데이터 처리 (예: SQLite, SharedPreferences 등)
      │   │   ├── 📂 dao/               # 데이터 접근 객체 (DAO)
      │   │   │   └── 📄 location_data_source.dart  # 위치 데이터 소스
      │   │   └── 📂 entity/            # 엔티티 클래스 (DB 모델 등)
      │   │       └── 📄 location_entity.dart       # 위치 엔티티
      │   ├── 📂 mapper/                # 데이터 매핑 관련 코드 (DTO -> Domain 모델 변환 등)
      │   │   ├── 📄 building_mapper.dart
      │   │   ├── 📄 construction_mapper.dart
      │   │   └── 📄 disability_support_center_mapper.dart
      │   ├── 📂 remote/                # 원격 데이터 처리 (API, 네트워크 호출 등)
      │   │   ├── 📂 api/               # API 통신 관련 코드
      │   │   │   ├── 📂 building/      # 건물 관련 API
      │   │   │   │   └── 📄 building_api.dart
      │   │   │   ├── 📂 construction/
      │   │   │   │   └── 📄 construction_api.dart
      │   │   │   ├── 📂 disability_support_center/
      │   │   │   │   └── 📄 disability_support_center_api.dart
      │   │   │   ├── 📂 disabled_restroom/
      │   │   │   │   └── 📄 disabled_restroom_api.dart
      │   │   │   ├── 📂 map/
      │   │   │   │   └── 📄 map_api.dart
      │   │   │   ├── 📂 navigation/
      │   │   │   │   └── 📄 navigation_api.dart
      │   │   │   ├── 📂 node/
      │   │   │   │   └── 📄 node_api.dart
      │   │   │   └── 📂 ramp/
      │   │   │       └── 📄 ramp_api.dart
      │   │   └── 📂 dto/               # 데이터 전송 객체 (DTO)
      │   │       ├── 📂 building/      # 건물 관련 DTO
      │   │       │   └── 📄 building_dto.dart
      │   │       ├── 📂 construction/
      │   │       │   └── 📄 construction_dto.dart
      │   │       ├── 📂 disability_support_center/
      │   │       │   └── 📄 disability_support_center_dto.dart
      │   │       ├── 📂 disabled_restroom/
      │   │       │   └── 📄 disabled_restroom_dto.dart
      │   │       ├── 📂 navigation/
      │   │       │   └── 📄 navigation_dto.dart
      │   │       ├── 📂 node/
      │   │       │   └── 📄 node_dto.dart
      │   │       └── 📂 ramp/
      │   │           └── 📄 ramp_dto.dart
      │   └── 📂 repository/            # 데이터 레포지토리 (데이터 처리 로직 포함)
      │       ├── 📄 place_repositoryImpl.dart
      │       └── 📄 school_repositoryImpl.dart
      │
      ├── 📂 di/                        # 의존성 주입 관련 코드
      │   ├── 📄 network_di.dart
      │   ├── 📄 place_di.dart
      │   ├── 📄 school_di.dart
      │   └── 📄 service_locator.dart
      │
      ├── 📂 domain/                    # 비즈니스 로직 (도메인 계층)
      │   ├── 📂 model/                 # 도메인 모델 (핵심 데이터 모델)
      │   │   ├── 📄 building.dart
      │   │   ├── 📄 construction_news.dart
      │   │   ├── 📄 place.dart
      │   │   └── 📄 support_center.dart
      │   ├── 📂 repository/            # 도메인 레포지토리 (비즈니스 로직을 다루는 추상화)
      │   │   ├── 📄 place_repository.dart
      │   │   └── 📄 school_repository.dart
      │   └── 📂 usecases/              # 유즈케이스 (실제 비즈니스 로직 실행)
      │       ├── 📄 add_saved_location_usecase.dart
      │       ├── 📄 check_if_place_saved_usecase.dart
      │       ├── 📄 delete_saved_location_usecase.dart
      │       ├── 📄 get_all_buildings_usecase.dart
      │       ├── 📄 get_all_construction_news_usecase.dart
      │       ├── 📄 get_all_support_centers_usecase.dart
      │       ├── 📄 get_building_detail_by_id.dart
      │       ├── 📄 get_building_name_by_id_usecase.dart
      │       ├── 📄 get_coordinate_by_nodeid_usecase.dart
      │       ├── 📄 get_latest_construction_news_usecase.dart
      │       ├── 📄 get_places_by_category_usecase.dart
      │       ├── 📄 get_place_by_name_usecase.dart
      │       ├── 📄 get_saved_locations_usecase.dart
      │       └── 📄 toggle_saved_location_usecase.dart
      │
      ├── 📂 navigation/                # 네비게이션 관련 코드
      │   └── 📄 main_navigation_page.dart
      │
      ├── 📂 presentation/              # 사용자 인터페이스(UI) 관련 코드
      │   ├── 📂 common/                # 공통 UI 컴포넌트
      │   │   ├── 📄 building_detail_popup.dart
      │   │   ├── 📄 category_chip.dart
      │   │   ├── 📄 category_list.dart
      │   │   ├── 📄 custom_btn.dart
      │   │   ├── 📄 custom_search_bar.dart
      │   │   ├── 📄 map_component.dart
      │   │   ├── 📄 place_item.dart
      │   │   ├── 📄 ramp_detail_popup.dart
      │   │   ├── 📄 restroom_detail_popup.dart
      │   │   └── 📄 route_finder_modal.dart
      │   ├── 📂 map/                   # 지도 관련 UI 코드
      │   │   ├── 📄 map_page.dart
      │   │   ├── 📄 search_page.dart
      │   │   └── 📄 search_viewmodel.dart
      │   ├── 📂 qa/                    # Q&A 관련 UI
      │   │   └── 📄 qa_page.dart
      │   ├── 📂 saved/                 # 저장된 항목들 관련 UI
      │   │   ├── 📂 componenet/        # 저장된 항목 관련 컴포넌트
      │   │   │   ├── 📄 saved_bottomsheet.dart
      │   │   │   └── 📄 saved_item.dart
      │   │   ├── 📄 save_page.dart
      │   │   └── 📄 save_viewmodel.dart
      │   ├── 📂 school/                # 학교 관련 UI
      │   │   ├── 📂 component/         # 학교 관련 컴포넌트
      │   │   │   ├── 📄 building_detail.dart
      │   │   │   ├── 📄 building_detail_viewmodel.dart
      │   │   │   ├── 📄 building_info_section.dart
      │   │   │   ├── 📄 building_info_viewmodel.dart
      │   │   │   ├── 📄 chacha_info_section.dart
      │   │   │   ├── 📄 construction_news_component.dart
      │   │   │   ├── 📄 construction_news_detail.dart
      │   │   │   ├── 📄 construction_news_viewmodel.dart
      │   │   │   ├── 📄 disabled_center_detail.dart
      │   │   │   ├── 📄 disabled_center_viewmodel.dart
      │   │   │   ├── 📄 lounge.dart
      │   │   │   ├── 📄 school_search_bar.dart
      │   │   │   └── 📄 section_title.dart
      │   │   └── 📄 school_page.dart
      │   └── 📂 theme/                 # 테마 및 스타일 관련 코드
      │       └── 📄 color.dart
      │
      └── 📄 main.dart                  # 앱의 진입점
    </details>
    
    - **BE 프로젝트 구조**
      <details>
      <summary>BE 프로젝트 구조 보기</summary>
    
      ```plaintext
      📂 src/
            ├── 📂 config/                      # 설정 관련 코드
            │   ├── 📄 app.rs                   # 앱 설정
            │   └── 📄 mod.rs                   # 설정 모듈
            │
            ├── 📂 db/                          # 데이터베이스 관련 코드
            │   ├── 📄 connection.rs            # DB 연결 설정
            │   └── 📄 mod.rs                   # DB 모듈
            │
            ├── 📂 handlers/                    # 요청 처리 핸들러
            │   ├── 📄 building.rs              # 건물 관련 요청 처리
            │   ├── 📄 construction_news.rs     # 공사 뉴스 요청 처리
            │   ├── 📄 disability_support_center.rs # 장애 지원 센터 요청 처리
            │   ├── 📄 disabled_restroom.rs     # 장애인 화장실 요청 처리
            │   ├── 📄 map.rs                   # 지도 요청 처리
            │   ├── 📄 mod.rs                   # 핸들러 모듈
            │   ├── 📄 navigation.rs            # 네비게이션 요청 처리
            │   └── 📄 ramp.rs                  # 경사로 요청 처리
            │
            ├── 📂 models/                      # 데이터 모델
            │   ├── 📄 building.rs              # 건물 모델
            │   ├── 📄 construction_news.rs     # 공사 뉴스 모델
            │   ├── 📄 disability_support_center.rs # 장애 지원 센터 모델
            │   ├── 📄 disabled_restroom.rs     # 장애인 화장실 모델
            │   ├── 📄 elevator.rs              # 엘리베이터 모델
            │   ├── 📄 mod.rs                   # 모델 모듈
            │   └── 📄 ramp.rs                  # 경사로 모델
            │
            ├── 📂 routes/                      # 라우팅 설정
            │   ├── 📄 building.rs              # 건물 라우트
            │   ├── 📄 construction_news.rs     # 공사 뉴스 라우트
            │   ├── 📄 disability_support_center.rs # 장애 지원 센터 라우트
            │   ├── 📄 disabled_restroom.rs     # 장애인 화장실 라우트
            │   ├── 📄 health.rs                # 헬스 체크 라우트
            │   ├── 📄 map.rs                   # 지도 라우트
            │   ├── 📄 mod.rs                   # 라우트 모듈
            │   ├── 📄 navigation.rs            # 네비게이션 라우트
            │   └── 📄 ramp.rs                  # 경사로 라우트
            │
            ├── 📂 services/                    # 서비스 로직
            │   ├── 📄 auth_service.rs          # 인증 서비스
            │   ├── 📄 mod.rs                   # 서비스 모듈
            │   └── 📄 user_service.rs          # 유저 서비스
            │
            ├── 📂 tests/                       # 테스트 코드
            │   ├── 📄 health.rs                # 헬스 체크 테스트
            │   ├── 📄 integration.rs           # 통합 테스트
            │   └── 📄 mod.rs                   # 테스트 모듈
            │
            ├── 📂 utils/                       # 유틸리티 코드
            │   ├── 📄 hasher.rs                # 해싱 유틸리티
            │   ├── 📄 logger.rs                # 로깅 유틸리티
            │   └── 📄 mod.rs                   # 유틸리티 모듈
            │
            ├── 📄 lib.rs                       # 라이브러리 모듈 진입점
            └── 📄 main.rs                      # 메인 프로그램 진입점
    </details>

4. **코드 사용 매뉴얼**
    | 😸 코드 사용 매뉴얼 |
    | --- |
    | [[moduCNU] 코드사용 매뉴얼.pdf](https://github.com/user-attachments/files/18406276/moduCNU.pdf) |

<br>

## 팀원

|<img src="https://avatars.githubusercontent.com/u/102356873?v=4" width="150" height="150"/>|<img src="https://avatars.githubusercontent.com/u/87114004?v=4" width="150" height="150"/>|<img src="https://avatars.githubusercontent.com/u/81271644?v=4" width="150" height="150"/>|
|:-:|:-:|:-:|
|[@aengzu](https://github.com/aengzu)|[@paintedblue](https://github.com/paintedblue)|[@Yijungu](https://github.com/Yijungu)|
| FE 주도 개발, 작업 문서 관리, 기획 | 기획 및 디자인 주도 개발, FE | BE 주도 개발 및 배포, FE, 기획, DB |
