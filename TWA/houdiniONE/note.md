# Houdini One

## EP7 Blicks 반복 작업을 위하여

1. input
1. 어떤 수행
1. 무엇에
1. 몇 번
1. 결과

fetch feedback    vs    fetch piece or point
by Count    vs    by piece or points
feedback    vs    merge

## EP8 Solver

시간에 따라
주어진 세팅에 맞게
작업을 수행해 주는 것

## Particle 01

> 1. principled shader 는 attribute vop 이다.
> 1. point 의 결과를 출력할 때 principled shader 를 쓰고 싶다면 primitive 로 치환하는 과정이 필요할수도 있다.
> 1. principled shader 가 적용되려면 primitive 가 필요하다.
> 1. 출력시 point 의 사이즈를 정하는 attribute 은 @pscale 이다
> 1. @metallic @rough 등으로 point 를 꾸며주고 싶다면 사용하는 material 이 @metallic @rough 등을 사용하는지 확인을 해야한다.
> 1. render 를 도와줄 유용한 material 이 있는 곳 material palette 는 유용한 attribute vop 들의 묶음이 있는 곳
> 1. 선을 출력할때 두께는 @width 로 조절한다
> 1. 선의 부피감을 주는 방법으로 poliwire, wireframe
> 1. 선의 색을 주는 방법으로 chramp() 를 이용했는데 이때 0~1의 값이 필요했기에 @u 를 만들어 선의 시작은 0 끝은 1로 표현, chramp 의 방식을 float 에서 color 로 변경 후 @u 에 색을 대응
> 1. 선의 alpha 값도 줄수있다

## Particle 02

위치 기반 Motion Blur (float frame 개념 필요 ($F → $FF))

1. Geo Time Samples
1. Xform time Samples

속도 기반 Motion Blur (Object 의 Render > Sampling > Velocity Blur)

## Particle 04

Particle 시스템이 구동이 되기위한 3가지 요소

- POP Object
- POP Source
- POP Solver

## Particle 05

```
@v += @gravity;
@P += @v;
```
