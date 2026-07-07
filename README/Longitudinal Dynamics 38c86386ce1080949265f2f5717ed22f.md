# Longitudinal Dynamics

## 예제 파일

[ex_longitudinal_force_balance.slx](Longitudinal%20Dynamics/ex_longitudinal_force_balance.slx)

[ex_longitudinal_total.slx](Longitudinal%20Dynamics/ex_longitudinal_total.slx)

[ex_longitudinal_with_vdb.slx](Longitudinal%20Dynamics/ex_longitudinal_with_vdb.slx)

- reference
    
    [차량동역학시뮬레이션+Lecture+#2 (1).pdf](Longitudinal%20Dynamics/%EC%B0%A8%EB%9F%89%EB%8F%99%EC%97%AD%ED%95%99%EC%8B%9C%EB%AE%AC%EB%A0%88%EC%9D%B4%EC%85%98Lecture2_(1).pdf)
    

# Force balance

![image.png](Longitudinal%20Dynamics/2e88b274-d7d3-4405-b4bd-eb26ba92cd8d.png)

<aside>
💡

힘 평형에 의해, 다음과 같이 정의된다.

$$
\Sigma F = m \overrightarrow a = 구동력 - 공기저항 - 구름저항 - 중력 \\ = F_x - (F_{aero} + R_x + mgsin\theta) \\ = (F_{xf} + F_{xr}) - (F_{aero}+(R_{xf} + R_{xr})+mgsin\theta) 
$$

즉, 차량의 바퀴를 굴리는 힘과 차량을 방해하는 힘의 합이 바로 차량의 종방향에 가해지는 힘이다.

</aside>

## 구동력

<aside>
💡

여기서는 구동력을 단순 힘으로 표현 (constant)

![image.png](Longitudinal%20Dynamics/image.png)

</aside>

## 공기저항

<aside>
💡

공기저항은 다음과 같이 표현할 수 있다.

$$
F_{aero} = -{{1}\over{2}} \rho C_d A_f (V_x + V_{wind}) ^2
$$

- 유체역학에서, 우리는 $F = {1\over2} \rho C_d A_f V^2$가 공기저항임을 배웠고, 여기에 바람의 속도 성분을 추가해준 식이다.
- Simulink 구현 모습
    
    ![image.png](Longitudinal%20Dynamics/image%201.png)
    
</aside>

## 구름저항

<aside>
💡

구름저항 모멘트는 다음과 같이 표현된다. (reference의 22페이지 아랫부분)

$$
M_{yRR} = F_z * R * Rr_{surf} * (Rr_c + Rr_v * v_x)
$$

이때, 모멘트는 힘 * 모멘트팔과 같이 구현되는데, 여기서 모멘트 팔은 Effective rolling radius이다. 즉, 구름저항은 다음과 같다.

$$
R_x = {M_{yRR}\over R} = F_z * Rr_{surf} * (Rr_c + Rr_v * v_x)
$$

- simulink 구현 모습
    
    ![image.png](Longitudinal%20Dynamics/image%202.png)
    
</aside>

## 중력

<aside>
💡

중력의 x방향 성분을 표현하면 다음과 같다.

$$
F_{grav} = mgsin\theta
$$

- simulink 구현 모습
    
    ![image.png](Longitudinal%20Dynamics/image%203.png)
    
</aside>

## 전체 구현 모습

![image.png](Longitudinal%20Dynamics/image%204.png)

<aside>
💡

차량의 종방향에 가해지는 힘은 아래와 같이 나타낸다.

$$
\Sigma F = m \overrightarrow a =F_x - (F_{aero} + R_x + mgsin\theta) \\ = (F_{xf} + F_{xr}) - (F_{aero}+(R_{xf} + R_{xr})+mgsin\theta) 
$$

즉, 차량의 가속도는 다음과 같다.

$$
a = {{F_x - (F_{aero}+R_x + mgsin\theta)} \over m} 
$$

가속도를 적분하면 차량의 속도, 그리고 속도를 적분하면 차량의 종방향 변위까지 알 수 있다.

- 가속도 결과 그래프
    
    ![image.png](Longitudinal%20Dynamics/image%205.png)
    
- 속도 결과 그래프
    - m/s 단위
        
        ![image.png](Longitudinal%20Dynamics/image%206.png)
        
    - km/h 단위
        
        ![image.png](Longitudinal%20Dynamics/image%207.png)
        
- 변위 결과 그래프
    
    ![image.png](Longitudinal%20Dynamics/image%208.png)
    
</aside>

# Moment balance

![image.png](Longitudinal%20Dynamics/c9fe75d4-4ba9-4dbb-a66f-bbb1be5cb42c.png)

<aside>
💡

모멘트 평형 관점에서 다음과 같이 표현된다. 
(이때, Rolling resistance는 타이어 접점 근처이므로 무시.)

$$
\Sigma M = I*\ddot\Phi |_{rear tire contact} = -F_{zf}(l_f+l_r) + mgl_rcos\theta - mghsin\theta - m\ddot xh - F_{aero} h_{aero} = 0
$$

즉, 차량에 가해지는 중력방향 힘은 다음과 같다.

$$
F_{zf} = {{mgl_rcos\theta - mghsin\theta - m\ddot x h -F_{aero} h_{aero}}\over L } \\ F_{zr} = {{mgl_fcos\theta + mghsin\theta + m\ddot x h +F_{aero} h_{aero}}\over L }
$$

</aside>

## Fzf

<aside>
💡

$$
F_{zf} = {{mgl_rcos\theta - mghsin\theta - m\ddot x h -F_{aero} h_{aero}}\over L }
$$

- simulink 구현 모습
    
    ![image.png](Longitudinal%20Dynamics/image%209.png)
    
</aside>

## Fzr

<aside>
💡

$$
F_{zr} = {{mgl_fcos\theta + mghsin\theta + m\ddot x h +F_{aero} h_{aero}}\over L }
$$

- simulink 구현 모습
    
    ![image.png](Longitudinal%20Dynamics/image%2010.png)
    
</aside>

# 모멘트 평형을 반영한 전체 구현 모습

## 힘 평형 Subsystem

![image.png](Longitudinal%20Dynamics/image%2011.png)

<aside>
💡

- 입력 (In) : 모멘트 평형의 결과로 나온 Fzf, Fzr
- 출력 (Out) : 질량, 가속도, 지면의 기울기, 공기저항력 (계산의 편의를 위함)
</aside>

## 모멘트 평형 Subsystem

![image.png](Longitudinal%20Dynamics/image%2012.png)

<aside>
💡

- 입력 : 질량, 가속도, 지면의 기울기, 공기저항력
- 출력 : Fzf, Fzr
</aside>

## 전체 System

![image.png](Longitudinal%20Dynamics/image%2013.png)

# Vehicle Dynamics Blockset 활용

![image.png](Longitudinal%20Dynamics/image%2014.png)

<aside>
💡

Vehicle Body 1DOF Longitudinal 블록을 활용한 종방향 모델 구현

- 입력
FwF : 전륜 구동력, FwR : 후륜 구동력
Grade : 지면의 기울기
WindX : 바람의 종방향 속도
- 출력
Info : 차량에 가해지는 다양한 물리량을 확인 가능
xdot : 차량의 종방향 속도
FzF : 차량 전륜에 가해지는 수직방향 힘, FzR : 차량 후륜에 가해지는 수직방향 힘
</aside>