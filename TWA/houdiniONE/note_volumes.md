# Houdini1 volumes

## 01

### Volume : 공간을 voxel 단위로 표현한 부피를 가진 결과물

- voxel(volume + pixel) : volume에서 이용되는 기본 단위

### Volume 의 분류

1. 결과로서의 Volume

    밀도에 의해 시각적 표현이 가능 : 연기, 구름, 불, 화염, 에너지

1. 정보 저장으로서의 Volume

    시각적으로는 큰 의미를 갖지 않음 : 바람, 곡률, normal

### Voxel 의 고유 Number

```text
    x:a y:b z:c

    x + (y*a) + (z*a*b)
```

### SDF : Signed Distance Field

### Volume 을 정의하기 위한 몇가지 사항

1. Volume 을 어디에 두고 싶은지 : Point 의 위치정보
1. 바운딩 박스가 존재 : 어느 정도의 규모에서 공간을 나눌지 정함
1. 어떻게 쪼개져 있을지 정해야 함. 그래야 voxel 의 사이즈가 정해짐
1. Volume 이 정의가 됐다면, 이 내용은 모두 Primitive 에 저장이 됨
1. 어떤 오브젝트 기준으로 Volume 을 만들려면 안과 밖을 구분해 줘야함. 이때 각각의 voxel 의 위치에서 오브젝트까지의 최단거리를 활용한다면 SDF 을 구할수 있음. Density 로도 Volume 을 표현할 수 있음
1. 정보의 효율을 위해서 그냥 Volume 이 아니라 VDB 포멧을 이용하기도 함

## 04

### Volume 관련 Node

- Name : Volume 의 이름을 바꿔줌
- Volume SDF : Fog Volume 을 SDF Volume 으로 바꿔줌
- Convert VDB

```text
Fog Volume  →(Volume SDF)→  SDF Volume
Fog Volume  →(Convert VDB)→  Fog VDB
Fog Volume  →(Convert VDB (VDB Class : Convert Fog to SDF))→  SDF VDB
Fog VDB  →(Convert VDB (VDB Class : Convert Fog to SDF))→  SDF VDB
Fog VDB  →(Convert VDB (Convert To : Volume))→  Fog Volume
SDF VDB  →(Convert VDB (Convert To : Volume))→  SDF Volume
Fog VDB  →(Convert VDB (Convert To : Volume))→(Volume SDF)→  SDF Volume
SDF VDB  →(Convert VDB (VDB Class : Convert SDF to Fog))→  Fog VDB
SDF Volume  →(Convert VDB)→  SDF VDB
- Volume  →(압축, 손실)→  VDB
- VDB  →(완벽할 수 없음)→  Volume
```

- Volume Merge : Volume 의 사칙연산을 쉽게 해줌, 기준이 되는 해상도가 자동으로 늘어남
- VDB  Combine
- Volume Blur
- VDB Smooth

## 5

1. vector volume & how to visualize
1. curvature & gradient
1. advect
1. volume with solver
1. particles with vector field

### Vector Volume 을 시각화

- Volume Slice
- Volume Trail

```text
Noise Function 의 진화
Periodic Noise(0~1) → Flow Noise → Anti-Aliased Noise(-1~1) → Anti-Aliased Flow Noise → Turbulent Noise(0~1) → Curl Noise (-1~1)

- 0~1 의 값을 반환하는 Noise : Scale
- -1~1 의 값을 반환하는 Noise : 공간
```

- Curl Noise : 공간상의 Noise 표현에 SDF 을 다룰수 있음, Surface Effect Radius 파라미터로 조정

### Curvature & Gradient

Curvature : 곡률의 크기 Float 값으로 이루어진 Volume 정보
Gradient : volume 의 Normal, SDF 가 커지는 흐름 방향

- VDB Analysis

Curvature  =  ± 1 / r (곡선에 접하는 구의 반지름)

>Volume 에서의 Curvature 필드는 SDF 가 같은 곳, Isovalue 가 같은 곳의 Curvature 값들을 보여주는 Volume 공간

1. Vector Volume
1. Vector Volume 시각화
1. Curl Noise : 공간상의 Noise 표현에 SDF 을 다룰수 있음
1. Volume 을 분석 (Gradient, Curvature)

## 6

Volume 에서 다룰 수 있는 정보의 형태

1. Float Volume
1. Vector Volume
1. Signed Distant Field
1. Gradient
1. Curvature

### advect

```text
이류 : 원본 Volume + 방향 + 이동(VDB Advect)
확산 : 원본 Volume + (방향 + 이동) + 값의 평형
```

Volume 의 Vector 정보를 확인할 때 Volume Trail 이용

```text
@P +=     @N     * @curvature;
           ↓            ↓
       @gradient   @curvature
           ↓ Vector Field (Volume VOP or Volume Wrangle)   
           VCB Advect
```

## 7

Solver 에서 VDB Advect 다루기

1. Solver Setting 을 잘 잡을 수 있는가?
1. 효과의 속도를 조절할 수 있는가? Timestep 의 값을 이해 했는가?
1. Vector Field 를 다룸에 있어 겁먹지 않고 정보들을 쓸 수 있는가?

Solver 에서 Fog Volume 다루기

- VDB Combine : VDB 합치기

## 8

불이나 연기에 대한 묘사에 필요한 것

1. 연기 : Density
1. 속도 (Vector Field : Advect 를 위해 필요함) : V or vel
1. 온도 : Temperature

DOP Network

- Smoke Object : 얻고 싶은 Volume 결과가 어떤 해상도, 어떤 이름을 가지고 싶은지 정함
- Volume Source : 어떤 자료를 불러서 Source 로 사용할 것인가 (density, temperature, v, vel, wind)
- Smoke Solver : 어떻게 계산을 할 것인가

Smoke Solver 작동 순서

- Source Volume (Geometry) → Target Field (DOP Network) → Solver

```text
Temperature → Vel → Density
Force(중력) → V(속도) → P(위치)
```

## 9

- Gas Dissipate : 밀도값 떨굼 (Evaporation Rate)
- Gas Turbulence : Noise Pattern 에 따른 추가의 Velocity 를 생성 (Scale)
- Gas Shred : Gradient 에 따른 추가의 Velocity 를 생성
- Gas Disturb : Random 하게 Velocity 를 파괴
- Gas Match Field : Field 를 복제
- Gas Linear Combination : Field 계산
- Gas Calculate : Field 계산
- Gas Analysis : Field 분석

### A. Smoke Solver

1. Source
1. Smoke Solver Setting
1. Shape
1. Tweak
1. File Caching

### B. POP Solver

1. Source
1. Solver Setting
1. V
1. Cache

- POP Advect by Volumes
