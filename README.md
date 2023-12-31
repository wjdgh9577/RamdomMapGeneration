# Random Map Generation(2020.07)

![제목 없는 동영상 - Clipchamp로 제작](https://github.com/wjdgh9577/Unity3D/assets/50287835/8ff3e27a-1bc9-44b0-abed-b08ad169dc77)

랜덤 맵 생성 알고리즘

<br/>

## Goal
회사의 개인 과제로, 맵 디자이너의 신규 맵 생산성을 올려줄 간단한 툴 제작이 목표
    
(실제 맵 디자이너는 복셀 하나하나를 직접 찍어서 맵을 만든다.)

<br/>

## Idea
BSP(Binary Space Partitioning)라는 로그라이크류 게임의 랜덤 맵 생성에 주로 쓰이는 알고리즘을 알게 됨.

내가 구현해야 될 랜덤 맵은 무작위의 언덕이 무조건 있어야 한다. (즉, 평지는 취급 안한다.)

랜덤 맵의 방 = 언덕이라고 생각하면 구현이 가능할 것 같다.

### BSP Algorithm
> 재귀적으로 유클리드 공간을 초평면 상의 볼록 집합으로 분할하는 기법.

한 공간을 두 공간으로 나눴을 때 두개의 방이 생겼다고 본다.

각 방은 다시 두개의 방으로 나뉠 수 있으며 이러한 분할은 트리의 구조를 갖는다.

<br/>

## Process
### 1. 3차원 배열 선언
가로, 세로, 높이가 각각 x, z, y인 3차원 배열을 선언한 후 값을 0으로 초기화.

배열의 값이 1인 경우 타일이 생성된 것으로 간주한다.

### 2. 가중치를 적용하여 맵의 1층 배치
y=0인 xz 평면을 위에서 바라봤을 때 원 또는 타원에 가까운 형태가 나오도록 가중치(0.1 or 1)를 설정.

### 3. 아래층의 데이터를 기반으로 3차원 배열 완성
아래층에 타일이 존재할 경우 가중치(weight)를 적용하여 타일 생성.

층이 높아질수록 가중치를 낮춰 언덕의 각이 자연스러워지도록 유도.

### 4. BSP 적용
BSP 클래스를 통해 8개의 방(언덕) 데이터를 수집한다.

(재귀함수의 depth에 따라 return되는 방의 수가 증가하지만 우리 게임의 맵에 그렇게 많은 언덕은 필요 없다.)

방의 분할비는 7:3 이내로 조절하여 언덕이 지나치게 뾰족하지 않게 한다.

언덕 데이터를 그대로 3차원 배열에 적용한다.

### 5. Smoothing
인접한 타일의 수(neighbourCount)가 일정 이하인 경우 타일을 제거한다.

맵의 외곽 타일이 제거되므로 결과적으로 언덕이 완만하게 깎이는 효과를 볼 수 있다.

### 6. Drawing
스무딩 된 배열의 타일을 출력한다.

<br/>

## Problem
기존의 방식은 방의 갯수가 적고 스무딩을 거칠수록 언덕이 분리돼서 골짜기의 표현이 거의 불가능했다.

따라서 골짜기를 표현하기 위한 방법을 생각해야 했다.

<br/>

## Solution
인접한 노드를 연결하는 것보다 단순히 BSP를 두 번 적용시키는 것이 골짜기 표현에 더 자연스러웠다.
