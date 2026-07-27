**Volume 03. AMR Sensors and Perception**




# Chapter 19. Obstacle Detection



## 19.1 Obstacle Definition for AMR

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

자율주행 모바일 로봇(Autonomous Mobile Robot, AMR) 시스템에서 장애물(Obstacle)이란 자율주행을 안전하게 수행하는 과정에서 로봇의 이동을 제한하거나 방해하거나 영향을 줄 수 있는 모든 물리적 객체(Physical Object), 환경 요소(Environmental Feature), 지형 조건(Terrain Condition), 동적 객체(Dynamic Entity)를 의미한다. 단순한 이동 로봇에서 장애물을 단순히 이동을 막는 물체로 정의했던 기존 개념과 달리, 현대의 AMR에서는 장애물이 인지(Perception), 위치추정(Localization), 경로 계획(Path Planning), 제어(Control), 안전(Safety), 운용 효율(Operation Efficiency), 임무 수행(Mission Execution)에 모두 영향을 미치는 요소로 정의된다. 따라서 장애물에 대한 이해는 단순한 충돌 회피(Collision Avoidance)를 넘어 지능적인 환경 해석(Environment Interpretation)과 자율 의사결정(Autonomous Decision Making)의 핵심 요소가 된다.



장애물의 정의는 로봇의 물리적 성능과 현재 수행하는 임무에 따라 달라진다. 소형 실내 배송 로봇에게는 이동을 완전히 막는 물체가 대형 실외 자율주행 플랫폼에서는 아무런 문제가 되지 않을 수도 있다. 또한 경사진 지면은 일반적인 물류 운송에서는 통과 가능한 지형일 수 있지만, 깨지기 쉬운 화물을 운반하거나 정밀 검사를 수행하는 상황에서는 위험한 장애물로 간주될 수 있다. 따라서 장애물은 항상 차량의 크기(Dimension), 이동 성능(Mobility), 적재 상태(Payload Condition), 안전 요구사항(Safety Requirement), 센서 성능(Sensor Capability), 임무 환경(Mission Context)에 따라 상대적으로 정의된다.



장애물 인지(Obstacle Perception)는 환경 센싱(Environmental Sensing)으로부터 시작된다. 라이다(LiDAR), 스테레오 카메라(Stereo Camera), 깊이 카메라(Depth Camera), 레이더(Radar), 초음파 센서(Ultrasonic Sensor), RGB 카메라(RGB Camera), 열화상 카메라(Thermal Camera), 관성측정장치(Inertial Measurement Unit, IMU), 휠 엔코더(Wheel Encoder), 위성항법시스템(Global Navigation Satellite System, GNSS)은 주변의 기하학적 구조와 움직임을 지속적으로 측정한다. 이러한 원시 센서 데이터는 점군(Point Cloud), 점유 격자(Occupancy Grid), 복셀 지도(Voxel Map), 의미 지도(Semantic Map), 고도 지도(Elevation Map), 객체 목록(Object List)과 같은 구조화된 환경 표현으로 변환된다. 따라서 장애물 검출은 자율주행을 위한 전체 인지 파이프라인(Perception Pipeline)의 한 단계에 해당한다.



정적 장애물(Static Obstacle)은 일반적인 로봇 운용 중 위치가 거의 변하지 않는 물체를 의미한다. 벽(Wall), 기둥(Column), 산업 장비(Machinery), 선반(Shelving System), 울타리(Fence), 영구적인 차단 시설(Permanent Barrier), 적재 도크(Loading Dock), 전기 캐비닛(Utility Cabinet), 랙(Storage Rack), 건물(Building), 기반 시설(Infrastructure) 등이 대표적인 예이다. 이러한 장애물은 장기 지도(Long-term Map)에 저장되어 위치추정을 위한 기준 특징(Reference Feature)으로 활용된다. 그러나 공장이나 창고, 건설 현장에서는 구조 변경이 발생할 수 있으므로 정적 장애물도 지속적인 감시와 갱신이 필요하다.



동적 장애물(Dynamic Obstacle)은 시간에 따라 지속적으로 위치가 변하는 객체이다. 보행자(Pedestrian), 지게차(Forklift), 다른 자율주행 로봇(Autonomous Robot), 사람이 운전하는 차량(Manually Driven Vehicle), 건설 장비(Construction Equipment), 드론(Drone), 이동식 산업 장비(Mobile Machinery), 움직이는 화물(Moving Inventory) 등이 이에 속한다. 동적 장애물은 실시간 검출(Real-time Detection), 객체 추적(Object Tracking), 움직임 추정(Motion Estimation), 이동 경로 예측(Trajectory Prediction), 불확실성 관리(Uncertainty Management), 지속적인 경로 재계획(Path Replanning)이 필요하다. 정적 장애물과 달리 장기 지도에 그대로 저장할 수 없으며, 미래 위치를 지속적으로 예측해야 한다.



임시 장애물(Temporary Obstacle)은 정적 장애물과 동적 장애물의 중간적인 성격을 가진다. 컨테이너(Container), 유지보수 장비(Maintenance Equipment), 주차된 차량(Parked Vehicle), 팔레트(Pallet), 건설 자재(Construction Material), 임시 펜스(Temporary Fence), 안전 바리케이드(Safety Barrier), 검사 장비(Inspection Tool), 이동식 작업대(Mobile Workstation) 등이 여기에 해당한다. 짧은 시간 동안은 정적인 것처럼 보이지만 수 시간 또는 수일 후에는 다른 위치로 이동될 수 있다. 따라서 장기 내비게이션 시스템은 이러한 객체를 영구 구조물과 구분하여 지도의 정확성을 유지해야 한다.



자연 장애물(Natural Obstacle)은 사람이 만든 구조물이 아니라 자연환경에서 발생하는 요소이다. 바위(Rock), 나무(Tree), 수풀(Bush), 식생(Vegetation), 쓰러진 나뭇가지(Fallen Branch), 물웅덩이(Puddle), 진흙(Mud), 눈(Snow), 모래(Sand), 울퉁불퉁한 지형(Uneven Terrain), 급경사(Steep Slope), 자갈(Loose Gravel), 고인 물(Water Accumulation)은 모두 자율주행에 영향을 준다. 실외 자율주행에서는 단순히 장애물을 검출하는 것보다 지형의 주행 가능성(Traversability)을 해석하는 것이 더욱 중요한 경우가 많다.



구조적 장애물(Structural Obstacle)은 독립된 객체라기보다 공간 구조 자체가 이동을 제한하는 경우를 의미한다. 낮은 천장(Low Ceiling), 천장 배관(Overhead Pipe), 좁은 출입문(Narrow Doorway), 교량(Bridge), 터널(Tunnel), 계단(Staircase), 경사로(Ramp), 천장에 매달린 장비(Overhanging Machinery), 공중 케이블(Suspended Cable), 비계(Scaffolding), 다층 플랫폼(Multi-level Platform) 등이 여기에 포함된다. 이러한 구조는 2차원 지도만으로는 표현하기 어렵기 때문에 3차원 인지(3D Perception)가 반드시 필요하다.



지형 기반 장애물(Terrain-related Obstacle)은 개별 물체가 아니라 지면의 특성 때문에 발생하는 장애물이다. 과도한 경사(Slope), 거친 표면(Roughness), 불안정한 지반(Instability), 미끄러운 노면(Slippery Surface), 연약 지반(Deformable Soil), 파손된 포장(Damaged Pavement), 포트홀(Pothole), 배수로(Drainage Channel), 자갈, 연약한 흙 등은 눈에 띄는 장애물이 없어도 안전한 주행을 어렵게 만든다. 따라서 최신 AMR은 단순한 점유 공간(Occupancy)뿐 아니라 고도 분석(Elevation Analysis), 표면 기하학(Surface Geometry), 의미 정보(Semantic Understanding), 차량 동역학(Vehicle Dynamics)을 이용하여 주행 가능성을 평가한다.



의미적 장애물(Semantic Obstacle)은 물리적인 형상뿐 아니라 운영상의 의미 때문에 접근이 제한되는 영역이다. 제한 구역(Restricted Zone), 위험 화학물질 보관소(Hazardous Chemical Storage), 고전압 설비(High-voltage Equipment), 비상구(Emergency Exit), 횡단보도(Pedestrian Crossing), 클린룸(Clean Room), 무균 구역(Sterile Environment), 보안 구역(Security Boundary), 적재 구역(Loading Area), 안전 제외 구역(Safety Exclusion Zone) 등이 여기에 해당한다. 의미 정보를 이해함으로써 로봇은 단순한 충돌 회피를 넘어 산업 규정과 운영 정책까지 준수할 수 있다.



가상 장애물(Virtual Obstacle)은 실제 물리적인 장애물이 없지만 소프트웨어적으로 접근을 제한하는 영역이다. 플릿 관리 시스템(Fleet Management System)은 가상 벽(Virtual Wall), 임시 제한 구역(Temporary Restricted Region), 작업 구역(Work Zone), 유지보수 구역(Maintenance Area), 교통 제어 구역(Traffic Control Zone), 저속 주행 구역(Speed Reduction Region), 비상 대피 통로(Emergency Evacuation Corridor)를 설정할 수 있다. 이러한 가상 장애물은 물리적 환경을 변경하지 않고도 로봇의 이동 정책을 유연하게 변경할 수 있기 때문에 창고, 공장, 병원, 물류 센터에서 매우 유용하게 활용된다.



움직이는 사람(Moving Human)은 모든 장애물 가운데 가장 높은 우선순위를 가진다. 사람의 움직임은 갑작스러운 정지, 방향 전환, 가속, 집단 행동, 주의 분산 등으로 인해 예측하기 어렵기 때문이다. 따라서 사람 중심 내비게이션(Human-aware Navigation)은 보행자 검출(Pedestrian Detection), 자세 추정(Body Pose Estimation), 의도 예측(Intention Prediction), 이동 경로 예측(Trajectory Forecasting), 사회적 내비게이션(Social Navigation), 적응형 안전 거리(Adaptive Safety Margin)를 함께 적용하여 사람과 로봇이 안전하고 자연스럽게 공존할 수 있도록 한다.



차량(Vehicle)은 사람과는 다른 움직임 특성을 가지므로 별도의 장애물 범주로 취급된다. 지게차, 트럭(Truck), 무인운반차(Automated Guided Vehicle, AGV), 서비스 차량(Service Vehicle), 건설 장비, 농업 기계(Agricultural Equipment), 다른 자율주행 로봇은 최소 회전 반경(Minimum Turning Radius), 가속 한계(Acceleration Limit), 제동 거리(Braking Distance), 진행 방향 등의 동역학적 특성을 가진다. 따라서 차량별로 서로 다른 운동 모델(Motion Model)을 적용하여 미래 움직임을 예측해야 한다.



장애물의 크기(Dimensions)는 단순한 점유 여부 이상의 의미를 가진다. 높이(Height), 폭(Width), 길이(Length), 부피(Volume), 방향(Orientation), 형상(Shape)은 로봇이 통과할 수 있는지, 아래로 지나갈 수 있는지, 우회해야 하는지를 결정한다. 가느다란 기둥(Thin Pole), 공중 케이블(Suspended Cable), 투명 유리벽(Transparent Barrier), 낮은 장애물(Low Obstacle), 복잡한 산업 장비는 큰 물체보다 오히려 검출이 어려운 경우가 많다. 따라서 정확한 3차원 기하학 모델링이 안전한 내비게이션을 위해 필수적이다.



장애물 분류(Obstacle Classification)는 검출된 환경 요소를 의미 있는 범주로 구분하는 과정이다. 단순히 모든 객체를 장애물로 처리하는 대신 사람, 차량, 산업 장비, 팔레트, 선반, 컨테이너, 식생, 기반 시설, 임시 장비, 지형 등으로 구분한다. 이러한 분류는 객체 종류에 따라 서로 다른 주행 정책을 적용할 수 있도록 한다. 예를 들어 사람 주변에서는 더 큰 안전 거리를 유지하고, 지게차에는 차량용 예측 모델을 적용하며, 고정 벽은 최소한의 회피만 수행할 수 있다.



장애물 검출 신뢰도(Obstacle Detection Confidence)는 자율주행 의사결정에서 매우 중요한 요소이다. 센서 노이즈, 악천후, 부분 가림, 조명 부족, 반사 표면, 부분 관측, 복잡한 환경은 모두 검출의 불확실성을 증가시킨다. 따라서 최신 인지 시스템은 단순한 검출 결과뿐 아니라 검출 확률과 신뢰도를 함께 계산한다. 내비게이션 시스템은 신뢰도가 낮아질수록 자동으로 안전 여유를 확대하여 보다 보수적인 주행을 수행한다.



장애물 추적(Obstacle Tracking)은 움직이는 객체의 동일성을 시간에 따라 유지하는 기술이다. 연속된 센서 프레임(Frame)에서 기하학적 유사성, 움직임 예측, 외형 특징(Appearance Feature), 확률 기반 데이터 연관(Data Association)을 이용하여 동일 객체를 연결한다. 이를 통해 속도 추정, 이동 경로 예측, 행동 분석, 충돌 위험 예측이 가능해진다. 추적 기능이 없으면 매 프레임마다 새로운 객체로 인식되어 미래 예측 능력이 크게 떨어진다.



부분 가림(Occlusion)은 장애물 인지를 매우 어렵게 만드는 요소이다. 차량, 선반, 건물, 식생, 산업 장비 등에 의해 중요한 객체가 일시적으로 가려질 수 있다. 따라서 로봇은 다중 시점(Multi-view), 시간적 융합(Temporal Fusion), 예측 추적(Predictive Tracking), 점유 추론(Occupancy Reasoning), 장면 완성(Scene Completion)을 이용하여 숨겨진 공간을 추론해야 한다. 그러나 관측되지 않은 공간에 대해서는 항상 적절한 불확실성을 유지해야 하며, 실제로 보지 못한 정보를 확정된 사실처럼 취급해서는 안 된다.



장애물의 중요도는 단순한 거리만으로 결정되지 않는다. 멀리 있지만 빠르게 접근하는 차량은 가까이에 있는 정지된 벽보다 훨씬 위험할 수 있다. 따라서 위험도 평가(Risk Assessment)는 장애물 위치(Position), 상대 속도(Relative Velocity), 미래 이동 경로(Predicted Trajectory), 충돌 확률(Collision Probability), 객체 종류(Object Category), 환경 상황(Environmental Context), 임무 중요도(Mission Urgency), 불확실성(Uncertainty)을 종합적으로 고려하여 수행된다. 지능형 내비게이션은 단순한 거리보다 실제 위험성을 기준으로 장애물의 우선순위를 결정한다.



장애물 표현(Obstacle Representation)은 응용 분야에 따라 달라진다. 단순한 시스템은 2차원 점유 격자나 이진 지도(Binary Map)를 사용할 수 있지만, 최신 자율주행 로봇은 3차원 점군(Point Cloud), 복셀 지도(Voxel Map), 의미 기반 점유 지도(Semantic Occupancy Grid), 다각형 경계(Polygon Boundary), 메시(Mesh), 부호 거리장(Signed Distance Field), 객체 중심 장면 그래프(Object-oriented Scene Graph), 학습 기반 환경 임베딩(Learned Environmental Embedding) 등을 활용한다. 이러한 표현 방식은 계산 효율, 메모리 사용량, 경로 계획 성능, 전체 자율주행 성능에 직접적인 영향을 미친다.



위치추정(Localization)은 장애물 해석에 크게 의존한다. 고정된 환경 구조물은 스캔 정합(Scan Matching), 특징 추출(Feature Extraction), 지도 정합(Map Registration)을 위한 기준 특징으로 사용된다. 반면 움직이는 장애물은 위치추정 과정에서 제외해야 한다. 따라서 인지 시스템은 지도 갱신 전에 영구 구조물과 임시 또는 동적 장애물을 정확하게 구분해야 한다.



경로 계획(Path Planning)은 장애물 정보를 이용하여 충돌 없는 경로를 생성하면서 이동 거리, 에너지 소비, 임무 시간, 안전성, 운용 효율을 함께 최적화한다. 전역 계획기(Global Planner)는 영구 구조물을 중심으로 계획을 수행하고, 지역 계획기(Local Planner)는 동적 장애물과 임시 변화, 새로운 센서 정보를 지속적으로 반영한다. 따라서 최신 경로 계획은 단순한 기하학적 충돌 검사뿐 아니라 장애물 예측, 불확실성, 의미 정보, 차량 동역학을 함께 고려한다.



장애물 회피(Obstacle Avoidance)는 단순한 긴급 충돌 방지를 의미하지 않는다. 부드러운 경로 수정(Smooth Trajectory Adjustment), 적응형 속도 제어(Adaptive Speed Regulation), 협력적 상호작용(Cooperative Interaction), 사회적으로 자연스러운 이동(Socially Acceptable Motion), 예측 제동(Predictive Braking), 동적 우회(Dynamic Rerouting), 사전 위험 감소(Proactive Risk Reduction)까지 모두 포함한다. 최신 자율주행 시스템은 충돌 직전에 반응하는 것이 아니라 미래 상황을 예측하여 미리 행동을 수정한다.



산업 환경(Industrial Environment)에서는 로봇 팔(Robotic Manipulator), 자동 생산 설비(Automated Production Equipment), 컨베이어 시스템(Conveyor System), 매달린 화물(Suspended Load), 천장 크레인(Overhead Crane), 회전 기계(Rotating Machinery), 자동문(Automated Door), 검사 플랫폼(Inspection Platform), 유지보수 작업(Maintenance Activity) 등 특수한 장애물이 존재한다. 이러한 장애물은 일정한 운용 주기를 가지는 경우가 많기 때문에 공장 정보 시스템과 연계하면 더욱 효율적인 경로 계획이 가능하다.



건설 환경(Construction Environment)은 가장 복잡한 장애물 환경 가운데 하나이다. 굴착 구역(Excavation), 임시 펜스, 건설 자재, 비계, 중장비, 작업자, 미완성 구조물, 울퉁불퉁한 지형, 먼지, 기상 변화가 지속적으로 발생한다. 따라서 건설 로봇은 장기간 운용 중에도 환경 변화를 지속적으로 반영할 수 있는 적응형 장애물 모델(Adaptive Obstacle Model)이 필요하다.



농업 로봇(Agricultural Robot)은 산업 환경과 전혀 다른 장애물을 다룬다. 작물(Crop), 나무(Tree), 관개 설비(Irrigation Equipment), 울타리(Fence), 동물(Animal), 진흙(Mud), 바위(Rock), 식생 밀도 변화, 울퉁불퉁한 지형, 계절에 따른 성장, 조명 변화 등이 모두 자율주행에 영향을 준다. 따라서 장애물의 정의도 작물의 성장 단계, 수확 작업, 기상 조건, 농장 관리 방식에 따라 달라진다.



장애물 데이터셋(Obstacle Dataset)은 강인한 인지 알고리즘 개발에 매우 중요한 역할을 한다. 자율주행 데이터셋, 산업 검사 데이터셋, 창고 데이터셋, 농업 데이터셋, 기업 자체 데이터셋에는 다양한 장애물 사례가 포함되어 있다. 그러나 특정 환경에서만 학습된 인공지능은 새로운 환경에서 성능이 크게 저하될 수 있으므로 다양한 환경과 계절, 운영 조건을 포함하는 데이터셋을 사용하는 것이 매우 중요하다.



성능 평가는 검출 정밀도(Detection Precision), 재현율(Recall), 평균 정밀도(Mean Average Precision, mAP), 위치 오차(Localization Error), 분류 정확도(Classification Accuracy), 추적 정확도(Tracking Accuracy), 움직임 예측 오차(Prediction Error), 지연 시간(Latency), 오검출(False Positive), 미검출(False Negative), 강인성(Robustness), 계산 효율(Computational Efficiency) 등을 이용하여 수행한다. 또한 개별 알고리즘보다 전체 자율주행 시스템의 성능 향상으로 이어지는지를 함께 평가해야 한다.



인공지능(AI)은 장애물 이해를 단순한 기하학적 검출에서 의미 기반 환경 추론(Semantic Environmental Reasoning)으로 발전시켰다. 심층 신경망(Deep Neural Network)은 다중 센서(Multimodal Sensor)로부터 풍부한 기하학 및 의미 정보를 학습하며, 트랜스포머(Transformer)는 여러 객체 사이의 장거리 공간 관계(Long-range Spatial Relationship)를 이해한다. 멀티모달 인지(Multimodal Perception)는 영상, 깊이, 라이다, 레이더, 언어(Language), 사전 환경 지식(Prior Environmental Knowledge)을 통합하여 더욱 복잡한 환경에서도 정확한 장애물 이해를 가능하게 한다.



파운데이션 모델(Foundation Model)은 앞으로 장애물의 개념 자체를 더욱 확장시킬 것으로 예상된다. 미래의 AMR은 단순히 장애물이 무엇인지만 인식하는 것이 아니라 왜 존재하는지, 앞으로 어떻게 움직일지, 사람이 어떻게 상호작용할지, 그리고 임무를 어떻게 변경해야 하는지까지 종합적으로 이해하게 될 것이다. 따라서 장애물은 단순한 충돌 대상이 아니라 풍부한 환경 맥락(Context)을 포함하는 지능적인 환경 요소(Environmental Entity)로 발전하게 될 것이다.



결국 현대 자율주행 모바일 로봇(AMR)에서 장애물의 정의는 단순한 물리적 충돌 회피를 훨씬 넘어선다. 장애물은 기하학적 구조(Geometric Structure), 의미적 제약(Semantic Constraint), 지형 특성(Terrain Characteristic), 운용 규칙(Operational Rule), 동적 객체(Dynamic Entity), 환경 불확실성(Environmental Uncertainty), 공간 관계(Contextual Relationship)를 모두 포함하는 통합적인 개념이다. 앞으로 로봇 기술이 파운데이션 모델과 지속적인 세계 모델(World Model) 기반으로 발전함에 따라 장애물 이해 역시 더욱 지능적인 환경 추론의 핵심 요소가 되어, 모든 자율주행 응용 분야에서 더욱 안전하고 적응적이며 지능적인 이동을 가능하게 하는 기반 기술로 발전하게 될 것이다.



## 19.2 Static Obstacle Detection

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

정적 장애물 검출(Static Obstacle Detection)은 자율주행 모바일 로봇(Autonomous Mobile Robot, AMR)의 안전한 이동을 방해할 수 있는 영구적이거나 고정된 물체를 식별하는 과정이다. 보행자나 차량과 같은 움직이는 객체를 지속적으로 감시하는 동적 장애물 검출(Dynamic Obstacle Detection)과 달리, 정적 장애물 검출은 일반적인 운용 중 위치가 변하지 않는 환경 구조물을 대상으로 한다. 벽(Wall), 기둥(Pillar), 선반(Storage Rack), 산업 장비(Machinery), 울타리(Fence), 적재 도크(Loading Dock), 작업대(Workstation), 출입문(Door), 컨테이너(Container), 고정 설비(Permanent Equipment), 건물 구조(Building Infrastructure) 등이 대표적인 대상이다. 이러한 정적 장애물을 안정적으로 검출하는 것은 위치추정(Localization), 지도작성(Mapping), 내비게이션(Navigation), 경로 계획(Path Planning), 장기 자율주행(Long-term Autonomous Operation)의 기하학적 기반을 제공한다.



정적 장애물 검출의 주요 목적은 단순히 물체를 인식하는 것이 아니라 환경에 대한 정확한 공간 이해(Spatial Understanding)를 구축하는 것이다. 인지 시스템은 장애물이 어디에 존재하는지, 어떤 기하학적 형태를 가지는지, 크기는 어떠한지, 주변 구조물과 어떤 관계를 가지는지를 파악해야 하며, 환경이 변경되었을 경우에는 이러한 정보를 지속적으로 갱신해야 한다. 정확한 정적 장애물 정보는 충돌 없는 경로 생성, 신뢰성 높은 위치추정 기준 제공, 운용 효율 향상, 장기간의 안전한 자율주행을 가능하게 한다.



정적 장애물 인지(Static Obstacle Perception)는 환경 센싱(Environmental Sensing)으로부터 시작된다. 라이다(LiDAR)는 주변 조명의 영향을 거의 받지 않으면서 매우 정확한 거리 정보를 제공하기 때문에 가장 널리 사용되는 센서이다. 스테레오 카메라(Stereo Camera)는 영상 대응(Image Correspondence)을 통해 깊이를 계산하고, 깊이 카메라(Depth Camera)는 근거리의 3차원 구조를 직접 측정한다. 레이더(Radar)는 악천후에서도 안정적인 장애물 검출을 지원하며, RGB 카메라(RGB Camera)는 풍부한 의미 정보(Semantic Information)를 제공한다. 각각의 센서는 장단점이 다르므로 다양한 환경에서 강인성을 확보하기 위해 여러 센서를 함께 사용하는 경우가 많다.



센서 동기화(Sensor Synchronization)는 정확한 정적 장애물 검출을 위한 필수 조건이다. 여러 센서에서 수집된 데이터는 서로 다른 샘플링 주기(Sampling Frequency)와 통신 지연(Communication Delay)을 가지더라도 동일한 물리적 장면을 나타내야 한다. 하드웨어 트리거(Hardware Trigger), 타임스탬프 동기화(Timestamp Synchronization), 네트워크 시간 동기화(Network Time Protocol), 움직임 보정(Motion Compensation)을 통해 라이다 스캔, 카메라 영상, 레이더 데이터, 관성 센서 정보가 기하학적으로 일관성을 유지하도록 한다. 이러한 동기화가 정확하지 않으면 환경 구조물이 왜곡되거나 잘못된 위치에 나타날 수 있다.



센서 보정(Calibration) 역시 매우 중요한 요소이다. 정확한 장애물 위치 계산은 센서들 사이의 기하학적 관계가 정확해야만 가능하다. 내부 보정(Intrinsic Calibration)은 센서 고유의 광학적 또는 측정 특성을 계산하며, 외부 보정(Extrinsic Calibration)은 각 센서와 로봇 좌표계(Robot Coordinate Frame) 사이의 강체 변환(Rigid Transformation)을 추정한다. 작은 보정 오차도 인지 과정 전체에서 누적되어 장애물 위치 오차, 지도 불일치, 위치추정 성능 저하, 내비게이션 성능 저하를 초래할 수 있다. 따라서 모든 자율주행 시스템에서 보정 검증은 반드시 수행되어야 하는 핵심 절차이다.



원시 센서 데이터(Raw Sensor Observation)는 장애물 추출 전에 다양한 전처리(Preprocessing)를 거친다. 노이즈 제거(Noise Filtering)는 센서 오차로 발생한 불필요한 데이터를 제거하고, 좌표 변환(Coordinate Transformation)은 모든 데이터를 하나의 기준 좌표계(Common Reference Frame)로 통합한다. 움직임 보정(Motion Compensation)은 로봇 이동에 따른 왜곡을 수정하며, 다운샘플링(Downsampling)은 계산량을 줄이고, 이상치 제거(Outlier Rejection)는 비정상적인 측정값을 제거한다. 적절한 전처리는 계산 효율을 향상시키는 동시에 장애물 검출에 필요한 핵심 기하학 정보를 유지한다.



점군(Point Cloud)은 정적 장애물 검출에서 가장 널리 사용되는 기하학적 표현이다. 각 점(Point)은 3차원 공간상의 위치를 나타내며, 경우에 따라 반사 강도(Intensity), 색상(Color), 타임스탬프(Timestamp), 의미 정보(Semantic Information), 신뢰도(Confidence) 등을 함께 포함한다. 점군은 환경의 기하학적 구조를 매우 정밀하게 표현할 수 있지만 수백만 개 이상의 점을 포함하는 경우가 많아 계산량이 매우 크다. 따라서 많은 시스템은 점군을 점유 격자(Occupancy Grid), 복셀(Voxel), 고도 지도(Elevation Map), 학습 기반 특징 표현(Learned Feature Representation)으로 변환하여 실시간 처리를 수행한다.



지면 분할(Ground Segmentation)은 정적 장애물 검출 이전에 일반적으로 수행되는 중요한 과정이다. 내비게이션 시스템은 이동 가능한 지면과 장애물을 구분해야 하기 때문이다. 알고리즘은 표면 기하학(Local Surface Geometry), 높이 연속성(Height Continuity), 곡률(Curvature), 주변 점의 관계(Neighborhood Relationship), 경사(Slope)를 분석하여 지면을 추출한다. 이후 남은 데이터는 주로 벽, 선반, 장비, 기둥, 산업 설비와 같은 수직 구조물에 해당하므로 장애물 검출이 훨씬 단순해지고 정확도가 향상된다.



점유 지도(Occupancy Mapping)는 공간의 각 영역이 장애물인지, 자유 공간(Free Space)인지, 또는 아직 충분히 관측되지 않았는지를 확률적으로 표현하는 방식이다. 베이지안 갱신(Bayesian Updating)을 이용하여 새로운 센서 데이터를 지속적으로 반영하면서도 부분적으로 관측된 영역의 불확실성을 유지한다. 점유 격자(Occupancy Grid)는 충돌 검사(Collision Checking)와 경로 계획(Path Planning)에 적합한 계산 효율성을 제공하며, 센서 노이즈나 불완전한 관측에도 안정적인 성능을 유지할 수 있다.



복셀 표현(Voxel Representation)은 점유 지도를 완전한 3차원 공간으로 확장한 방식이다. 2차원 점유 지도와 달리 천장 장애물(Overhead Obstacle), 매달린 장비(Suspended Equipment), 교량(Bridge), 터널(Tunnel), 다층 선반(Multilevel Shelving), 계단(Staircase), 배관(Pipeline), 복잡한 산업 시설(Industrial Facility)까지 표현할 수 있다. 로봇의 높이, 적재물의 크기, 센서 위치, 상부 여유 공간(Overhead Clearance)이 내비게이션에 영향을 미치는 환경에서는 복셀 기반 표현이 특히 중요하다.



기하학적 특징 추출(Geometric Feature Extraction)은 장애물을 보다 효과적으로 표현하기 위해 환경의 대표적인 구조를 찾는 과정이다. 평면(Plane)은 벽, 바닥, 천장을 나타내고, 선(Line Feature)은 모서리, 빔(Beam), 파이프(Pipe), 구조 경계를 표현한다. 코너(Corner)는 건물의 모서리나 장비의 경계를 나타내며, 표면 법선(Surface Normal), 곡률(Curvature), 지역 기하학 특징(Local Geometric Descriptor)은 장애물의 형상과 방향을 추가적으로 설명한다. 이러한 특징들은 위치추정, 지도작성, 객체 인식, 구조 분석에서 중요한 역할을 수행한다.



장애물 분할(Obstacle Segmentation)은 주변 환경으로부터 개별 구조물을 분리하는 과정이다. 영역 확장(Region Growing)은 유사한 기하학적 특성을 가진 점들을 하나의 그룹으로 묶으며, 유클리드 클러스터링(Euclidean Clustering)은 공간적으로 분리된 구조물을 독립적으로 구분한다. 연결 요소 분석(Connected-component Analysis)은 연속된 점유 영역을 찾고, 그래프 기반 분할(Graph-based Segmentation)은 주변 관계를 이용하여 구조물을 분리한다. 성공적인 분할은 벽, 기둥, 선반, 장비, 컨테이너 등을 각각 독립적으로 분석할 수 있도록 해준다.



장애물 분류(Obstacle Classification)는 기하학적 검출 결과에 의미 정보(Semantic Meaning)를 추가하는 과정이다. 단순히 점유 공간으로 표현하는 것이 아니라 벽, 선반, 산업 장비, 작업대, 전기 캐비닛(Electrical Cabinet), 적재 도크, 안전 펜스(Safety Barrier), 계단, 출입문, 기둥, 건물 구조물 등으로 분류한다. 이러한 의미 정보는 단순한 충돌 회피를 넘어 산업 규칙, 유지보수 요구사항, 작업 환경 맥락(Context)을 반영하는 지능적인 내비게이션을 가능하게 한다.



3차원 경계 상자(3D Bounding Box)는 검출된 장애물을 간결하게 표현하는 대표적인 방법이다. 객체의 위치(Position), 크기(Dimensions), 방향(Orientation), 신뢰도(Confidence)를 계산하여 저장한다. 복잡한 산업 구조물은 더욱 정밀한 기하학 모델이 필요할 수도 있지만, 경계 상자는 경로 계획과 충돌 검사에서 계산량을 크게 줄여준다. 또한 메시(Mesh), 다각형(Polygon), 볼록 껍질(Convex Hull), 부호 거리장(Signed Distance Field), 표면 모델(Surface Model) 등을 함께 사용하면 조작(Manipulation), 검사(Inspection), 정밀 내비게이션에서도 더욱 높은 기하학적 정확도를 확보할 수 있다.



구조적 장애물(Structural Obstacle)은 일반적으로 규칙적인 기하학적 형태를 가지므로 인식이 비교적 용이하다. 산업 현장의 벽은 넓은 평면으로 구성되고, 선반은 반복적인 수직·수평 구조를 가지며, 기계는 직육면체 형태가 많고, 기둥은 원통형 구조를 가지는 경우가 많다. 이러한 구조적 특성을 활용하면 단순한 데이터 기반 분할보다 더 높은 정확도와 계산 효율을 달성할 수 있다.



지도 일관성(Map Consistency)은 정적 장애물 검출에서 매우 중요한 요소이다. 환경 지도는 여러 차례의 주행에서 수집한 센서 데이터를 누적하여 점차 완성도를 높이고 노이즈를 감소시킨다. 그러나 동시에 새롭게 설치된 장비, 이동된 기계, 제거된 선반, 건물 구조 변경과 같은 실제 환경 변화도 감지해야 한다. 따라서 장기 지도(Long-term Map)는 안정성을 유지하면서도 환경 변화를 지속적으로 반영해야 한다.



위치추정(Localization)은 정적 장애물에 크게 의존한다. 영구적인 환경 구조물은 스캔 정합(Scan Matching), 특징 추출(Feature Extraction), 지도 정합(Map Registration)을 위한 기준 특징(Landmark)으로 사용된다. 반면 움직이는 장애물은 위치추정 정확도를 저하시킬 수 있으므로 일반적으로 제외된다. 따라서 정적 구조물과 임시 또는 동적 객체를 정확히 구분하는 것은 장기 자율주행의 위치추정 정확도를 크게 향상시킨다.



경로 계획(Path Planning)은 정적 장애물 정보를 이용하여 전역(Global) 차원의 최적 경로를 생성한다. 영구적인 구조물은 이동 가능한 공간의 기본 형태를 결정하므로 전역 계획기(Global Planner)는 주로 정적 환경 정보를 사용하여 최적 경로를 계산한다. 이후 지역 계획기(Local Planner)는 동적 장애물을 반영하여 실시간으로 경로를 수정한다. 따라서 정적 장애물 검출은 장거리 임무 계획과 실시간 내비게이션을 모두 지원하는 안정적인 공간 구조를 제공한다.



산업 환경(Industrial Environment)은 정적 장애물 검출에 많은 도전 과제를 제공한다. 밀집된 산업 장비, 좁은 통로, 반사 표면, 반복적인 구조, 지속적으로 변화하는 설비 구성 등이 복잡한 기하학적 환경을 형성한다. 선반, 컨베이어 시스템(Conveyor System), 로봇 팔(Robotic Manipulator), 공작 기계(Machine Tool), 검사 설비, 생산 라인, 전기 캐비닛, 안전 펜스, 각종 유틸리티 설비는 서로 매우 가까이 배치되어 있으므로 강인한 인지 알고리즘이 필요하다.



창고 환경(Warehouse Environment)은 선반, 랙, 팔레트, 적재 구역, 도킹 스테이션(Docking Station), 충전 스테이션(Charging Station), 기둥, 안전 펜스, 건물 구조물 등을 정확하게 검출하는 것이 매우 중요하다. 높은 선반은 부분 가림(Occlusion)을 발생시키며 반복적인 구조는 위치추정을 어렵게 만든다. 3차원 인지는 영구적인 저장 시설과 일시적인 화물을 구분하여 지속적인 물류 작업 중에도 안정적인 자율주행을 가능하게 한다.



건설 환경(Construction Environment)은 시간이 지남에 따라 구조 자체가 변화하기 때문에 더욱 복잡하다. 임시 벽, 비계(Scaffolding), 건설 자재, 미완성 건물, 굴착 구역, 장비 적치 공간 등은 시간이 지나면서 계속 변경된다. 따라서 건설 로봇의 인지 시스템은 완전히 고정된 구조물과 일정 기간 유지되는 반영구 구조물을 구분하면서 지속적으로 환경을 갱신해야 한다.



실외 환경(Outdoor Environment)은 바위(Rock), 나무(Tree), 식생(Vegetation), 옹벽(Retaining Wall), 배수 시설(Drainage System), 전신주(Utility Pole), 울타리(Fence), 도로 방호벽(Roadside Barrier), 제방(Embankment), 교량(Bridge), 다양한 지형 요소(Terrain Feature)를 정적 장애물로 인식해야 한다. 또한 비, 안개, 눈, 먼지, 조명 변화, 계절 변화는 센서 성능에 큰 영향을 미친다. 따라서 실외에서는 라이다, 레이더, 카메라, GNSS, 관성 센서를 함께 사용하는 다중 센서 융합(Multi-sensor Fusion)이 일반적으로 적용된다.



인공지능(AI)은 정적 장애물 검출 성능을 크게 향상시켰다. 점 기반 신경망(Point-based Network)은 점군을 직접 처리하고, 복셀 기반 신경망(Voxel-based Network)은 희소 3차원 합성곱(Sparse 3D Convolution)을 수행하며, 조감도(Bird\'s-Eye View, BEV) 기반 모델은 계산 효율을 높인다. 또한 트랜스포머(Transformer)는 장거리 공간 관계(Long-range Spatial Relationship)를 학습할 수 있어 복잡한 환경에서도 기존의 수작업 기하학 특징보다 우수한 성능을 제공한다.



의미 분할(Semantic Segmentation)은 정적 장애물 이해를 더욱 풍부하게 만든다. 모든 점, 픽셀, 복셀에 의미 정보를 부여하여 벽, 바닥, 천장, 산업 장비, 선반, 출입문, 창문, 배관, 전기 설비, 안전 구조물 등을 구분한다. 이러한 조밀한(Dense) 의미 정보는 내비게이션뿐 아니라 유지보수(Maintenance), 산업 검사(Inspection), 시설 관리(Facility Management), 자율 의사결정에도 활용될 수 있다.



다중 센서 융합(Multi-sensor Fusion)은 서로 다른 센서의 장점을 결합하여 검출 성능을 향상시킨다. 라이다는 정확한 기하학 정보를 제공하고, 카메라는 외형과 의미 정보를 제공하며, 레이더는 악천후에서도 안정적으로 동작하고, 깊이 카메라는 근거리 정밀 인지를 제공하며, 관성 센서는 환경 정합을 안정화한다. 특징 수준 융합(Feature-level Fusion), 객체 수준 융합(Object-level Fusion), 확률 기반 융합(Probabilistic Fusion)은 각각의 센서가 가진 약점을 보완하여 다양한 환경에서도 높은 인지 성능을 유지하도록 한다.



부분 가림(Occlusion)은 정적 장애물 검출에서 가장 어려운 문제 가운데 하나이다. 벽, 산업 장비, 선반, 컨테이너, 각종 구조물이 주변 환경 일부를 가릴 수 있다. 다중 시점 관측(Multi-view Observation), 시간적 융합(Temporal Integration), 지도 누적(Map Accumulation), 장면 완성(Scene Completion)은 가려진 구조를 복원하는 데 도움을 준다. 그러나 실제로 관측되지 않은 공간에 대해서는 항상 불확실성을 유지해야 하며, 완전한 환경 정보를 알고 있다고 가정해서는 안 된다.



실시간 구현(Real-time Implementation)은 인지 정확도와 계산 효율 사이의 균형을 요구한다. 대규모 점군, 복셀 지도, 의미 분할, 객체 분류, 위치추정 지원, 지도 갱신을 모두 제한된 시간 안에 수행해야 한다. GPU 가속(GPU Acceleration), 희소 계산(Sparse Computation), 적응형 해상도(Adaptive Resolution), 계층적 처리(Hierarchical Processing), 비동기 파이프라인(Asynchronous Pipeline), 모델 최적화(Model Optimization)는 임베디드 로봇 플랫폼에서도 안정적인 실시간 장애물 검출을 가능하게 한다.



성능 평가는 기하학적 정확도(Geometric Accuracy), 검출 정밀도(Detection Precision), 재현율(Recall), 위치 오차(Localization Error), 분할 품질(Segmentation Quality), 지도 일관성(Map Consistency), 계산 지연 시간(Computational Latency), 메모리 사용량(Memory Consumption), 강인성(Robustness), 장기 안정성(Long-term Stability)을 종합적으로 평가한다. 또한 다양한 환경 조건, 센서 구성, 이동 속도, 조명 변화, 구조적 복잡성을 포함한 반복 시험을 수행하여 실제 장기 자율주행에서도 안정적인 성능을 유지하는지를 검증해야 한다.



미래의 정적 장애물 검출(Static Obstacle Detection)은 파운데이션 모델(Foundation Model), 세계 모델(World Model), 의미 지도(Semantic Mapping), 디지털 트윈(Digital Twin), 평생 학습(Lifelong Learning), 맥락 기반 추론(Contextual Reasoning)을 통합하는 방향으로 발전할 것이다. 미래의 AMR은 단순히 정적인 물체를 검출하는 수준을 넘어 구조물의 기능(Function), 운영상의 의미(Operational Significance), 유지보수 이력(Maintenance History), 향후 환경 변화(Expected Environmental Evolution), 구조물 간의 관계(Relationship)를 함께 이해하게 될 것이다. 따라서 정적 장애물 검출은 단순한 기하학적 센싱을 넘어 보다 높은 수준의 자율 추론, 적응형 내비게이션, 지능형 의사결정을 지원하는 종합적인 환경 이해(Environmental Understanding) 기술로 발전하게 될 것이다.



## 19.3 Dynamic Obstacle Detection

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

동적 장애물 감지(동적 장애물 감지, Dynamic Obstacle Detection)는 자율주행 모바일 로봇(Autonomous Mobile Robot, AMR)에서 가장 중요한 인지(Perception) 기능 중 하나이다. 주변 환경은 사람, 차량, 로봇, 산업 장비 등이 지속적으로 이동하면서 끊임없이 변화하기 때문에, 시스템은 이러한 변화를 실시간으로 인식해야 한다. 정적 장애물 감지(Static Obstacle Detection)가 위치가 변하지 않는 물체를 식별하는 데 초점을 맞춘다면, 동적 장애물 감지는 물체의 움직임을 인식하고, 속도를 추정하며, 미래 이동 경로를 예측하고, 환경 정보를 지속적으로 갱신해야 한다. 따라서 인지 시스템은 안전한 자율주행을 유지하면서도 불필요한 정지를 최소화하기 위해 정확한 센서 계측, 강인한 추적(Object Tracking), 그리고 저지연(Low Latency) 처리 능력을 동시에 갖추어야 한다.



동적 장애물(Dynamic Obstacle)은 로봇이 운행하는 동안 위치가 변화하는 모든 물체를 의미한다. 대표적인 예로는 물류창고 통로를 걷는 작업자, 교차로를 통과하는 지게차(Forklift), 동일한 공간에서 운행하는 다른 자율주행 로봇, 사람이 밀고 다니는 카트, 자전거, 배송 차량, 건설 장비, 그리고 실외 환경으로 진입하는 동물 등이 있다. 일부 장애물은 비교적 예측 가능한 움직임을 보이지만, 다른 장애물은 매우 불규칙한 행동을 나타내므로 동적 장애물 감지는 정적 물체를 감지하는 것보다 훨씬 어려운 문제이다.



동적 장애물 감지의 첫 번째 단계는 센서를 이용한 지속적인 환경 관측이다. LiDAR는 반복적으로 주변 환경을 스캔하고 연속된 포인트 클라우드(Point Cloud)를 비교하여 움직임을 탐지한다. RGB 카메라는 연속된 영상에서 광류(Optical Flow)를 계산하여 이동하는 영역을 검출한다. 레이더(Radar)는 도플러(Doppler) 정보를 이용하여 상대 속도를 직접 측정하므로 고속 실외 주행에서 매우 유용하다. 깊이 카메라(Depth Camera)는 근거리 이동 물체에 대한 밀집된 3차원 정보를 제공하며, 특히 실내 환경에서 효과적인 역할을 수행한다.



시간적 연속성(Temporal Consistency)은 움직임 감지의 핵심 원리이다. 하나의 센서 프레임(Frame)은 단순히 특정 시점의 환경만 보여주지만, 여러 개의 시간적으로 연속된 프레임을 분석하면 물체의 이동 패턴을 파악할 수 있다. 인지 시스템은 시간에 따라 수집된 데이터를 비교하여 물체가 정지해 있는지, 이동하고 있는지를 판단한다. 이를 위해서는 센서 간 타임스탬프(Time Stamp)가 정확하게 동기화되어야 하며, 작은 시간 오차도 잘못된 속도 계산이나 허위 움직임(False Motion)을 발생시킬 수 있다.



객체 검출(Object Detection) 알고리즘은 각 센서 프레임에서 잠재적인 장애물을 먼저 식별한다. 딥러닝(Deep Learning) 기반 모델은 사람, 차량, 지게차, 자전거, 팔레트(Pallet) 등을 외형 특징을 이용하여 검출하며, LiDAR 기반 클러스터링(Clustering) 알고리즘은 포인트 클라우드에서 기하학적 구조를 분석하여 물체를 분리한다. 객체가 검출되면 이후 추적 과정에서 동일한 물체를 계속 식별할 수 있도록 임시 식별자(Temporary Identity)가 부여된다.



객체 추적(객체 추적, Object Tracking)은 개별 검출 결과를 연속적인 움직임 이력으로 변환하는 과정이다. 추적 알고리즘은 물체의 위치, 속도, 가속도, 이동 방향을 지속적으로 추정하면서 동일한 객체의 식별 정보를 유지한다. 칼만 필터(Kalman Filter), 확장 칼만 필터(Extended Kalman Filter), 비선형 칼만 필터(Unscented Kalman Filter), 파티클 필터(Particle Filter), 다중 가설 추적(Multiple Hypothesis Tracking) 등이 측정 오차가 존재하는 환경에서 객체의 상태를 안정적으로 추정하기 위해 널리 사용된다.



다중 객체 추적(Multi-Object Tracking)은 작업자, 로봇, 차량이 동시에 움직이는 복잡한 산업 환경에서 매우 중요하다. 시스템은 모든 객체에 고유 식별자(ID)를 부여하고, 일시적인 가림(Occlusion), 교차 이동, 부분적인 시야 손실이 발생하더라도 동일한 객체를 지속적으로 추적해야 한다. 안정적인 식별자 유지 기능은 경로 계획(Path Planning) 과정에서 서로 다른 객체를 혼동하여 발생하는 오류를 방지한다.



속도 추정(속도 추정, Velocity Estimation)은 동적 장애물 감지에서 가장 중요한 출력 정보 중 하나이다. 시스템은 단순히 물체의 위치만 계산하는 것이 아니라 로봇에 대한 상대 속도와 이동 방향까지 추정한다. 이러한 속도 정보는 연속적인 LiDAR 스캔, 카메라 기반 광류, 레이더의 도플러 측정, 또는 다양한 센서를 융합한 센서 융합(Sensor Fusion) 알고리즘을 통해 계산된다. 정확한 속도 추정은 충돌 이후 대응하는 것이 아니라 충돌을 사전에 예측하고 회피하는 예측 기반 안전 제어를 가능하게 한다.



궤적 예측(궤적 예측, Trajectory Prediction)은 현재의 속도 정보를 이용하여 미래의 이동 경로를 예측하는 과정이다. 단순한 모델은 일정한 속도를 가정하지만, 보다 발전된 모델은 가속도, 회전 행동, 도로 구조, 의미 기반 장면 이해(Scene Understanding), 학습된 행동 패턴 등을 함께 고려한다. 사람은 이동 방향을 갑자기 변경하는 경우가 많으므로 확률 기반 모델(Probabilistic Model)이 자주 사용된다. 산업용 차량은 통행 경로와 작업 규칙이 비교적 명확하기 때문에 보다 안정적인 예측이 가능하다.



동적 장애물 분류(Dynamic Obstacle Classification)는 내비게이션 시스템이 적절한 회피 전략을 선택할 수 있도록 지원한다. 사람은 움직임을 예측하기 어렵고 안전 규정상 충분한 보호 거리를 유지해야 하므로 가장 보수적인 안전 여유가 요구된다. 지게차는 일반적으로 정해진 작업 경로를 따르지만 화물을 적재하거나 하역하는 과정에서 갑자기 정지할 수 있다. 다른 자율주행 로봇은 자신의 이동 계획을 공유할 수 있으므로 플릿 관리(Fleet Management) 시스템을 통해 더욱 협조적인 경로 계획이 가능하다.



센서 융합(센서 융합, Sensor Fusion)은 동적 장애물 감지의 신뢰성을 크게 향상시킨다. LiDAR는 정확한 거리와 기하학 정보를 제공하고, 카메라는 의미 기반 객체 분류를 수행하며, 레이더는 악천후 환경에서도 안정적인 속도 측정을 제공한다. 초음파 센서(Ultrasonic Sensor)는 근거리 사각지대를 보호한다. 이러한 다양한 센서 정보를 통합하면 개별 센서의 한계를 보완하면서 오검출(False Detection)을 줄이고 다양한 환경에서 더욱 안정적인 인지 성능을 확보할 수 있다.



실외 환경은 실내보다 훨씬 복잡한 동적 장애물을 포함한다. 자동차, 오토바이, 자전거, 보행자, 건설 장비, 야생동물 등이 동시에 존재할 수 있으며, 비, 안개, 눈, 먼지, 강한 햇빛, 울퉁불퉁한 지형과 같은 환경 조건도 움직임 추정을 어렵게 만든다. 따라서 실외 자율주행 로봇은 일반적으로 LiDAR, 레이더, 카메라, GNSS(Global Navigation Satellite System), IMU(Inertial Measurement Unit)를 동시에 활용하는 다중 센서 인지 구조를 사용한다.



가림 처리(Occlusion Handling)는 동적 장애물 감지에서 가장 어려운 문제 중 하나이다. 이동 중인 객체는 선반, 주차된 차량, 벽, 산업 장비 또는 다른 작업자 뒤로 일시적으로 사라질 수 있다. 강인한 추적 알고리즘은 이러한 일시적인 시야 손실 동안에도 움직임 모델을 이용하여 객체의 위치를 계속 예측한다. 객체가 다시 나타나면 데이터 연관(Data Association) 알고리즘은 새로운 관측값을 기존 객체와 연결하여 중복 객체가 생성되는 것을 방지한다.



오검출 억제(False Positive Suppression) 또한 매우 중요하다. 불필요한 장애물 검출은 로봇의 생산성을 크게 저하시킬 수 있기 때문이다. 센서 반사, 움직이는 그림자, 빗방울, 흔들리는 나뭇가지, 센서 진동, 순간적인 측정 노이즈 등이 이동 장애물처럼 인식될 수 있다. 신뢰도 평가(Confidence Estimation), 시간적 필터링(Temporal Filtering), 센서 간 상호 검증, AI 기반 분류 기법은 이러한 허위 검출을 줄이면서도 높은 안전성을 유지하도록 지원한다.



실시간 처리 요구사항(Real-Time Processing Requirements)은 동적 장애물 감지 시스템에 상당한 계산 부하를 발생시킨다. 인지 시스템은 고주파 센서 데이터를 지속적으로 처리하면서도 전체 지연 시간을 매우 낮게 유지해야 한다. 일반적인 산업용 로봇은 10Hz에서 30Hz 수준의 인지 업데이트 주기를 요구하며, 고속으로 주행하는 실외 플랫폼은 긴 제동 거리를 고려하여 이보다 더 높은 업데이트 주기를 요구하는 경우도 많다.



검출된 장애물 정보는 내비게이션 모듈이 활용할 수 있는 표준화된 환경 표현(Environment Representation)으로 변환된다. 여기에는 객체 위치, 속도 벡터, 예측 궤적, 신뢰도, 객체 종류, 추적 식별자, 경계 상자(Bounding Box) 크기, 그리고 불확실성(Uncertainty) 정보가 포함된다. 지역 경로 계획기(Local Path Planner)는 이러한 정보를 이용하여 로봇의 운동학적 제약과 안전 요구사항을 만족하는 충돌 없는 이동 경로를 생성한다.



동적 장애물 정보는 단순한 기하학적 경로 계획뿐 아니라 행동 계획(Behavior Planning)에도 직접 영향을 미친다. 내비게이션 시스템은 현재 경로를 유지할지, 속도를 줄일지, 일시 정지할지, 교차하는 차량이나 작업자에게 양보할지, 느린 장애물을 추월할지, 또는 혼잡한 구간을 우회할지를 판단한다. 따라서 상위 수준의 행동 의사결정은 주변 동적 객체를 얼마나 정확하게 이해하는지에 크게 의존한다.



안전 통합(Safety Integration)을 위해서는 인지 시스템과 기능 안전(Functional Safety) 시스템이 긴밀하게 연동되어야 한다. 독립적으로 인증된 안전 LiDAR(Safety LiDAR)는 AI 인지 시스템과 관계없이 보호 구역(Protective Field)을 감시하여 긴급 정지를 수행할 수 있다. 반면 AI 기반 동적 장애물 감지는 더욱 풍부한 의미 정보를 제공하여 운영 효율성을 높이지만, 인증된 안전 기능을 대체하지는 않는다. 이러한 계층형 구조는 규제 요구사항을 만족하면서도 지능적인 자율주행을 동시에 가능하게 한다.



동적 장애물 감지의 성능 평가는 여러 가지 지표를 함께 사용한다. 검출 정확도(Detection Accuracy)는 이동 객체를 얼마나 정확하게 인식하는지를 평가하며, 추적 성능(Tracking Metrics)은 동일 객체의 식별 정보를 얼마나 안정적으로 유지하는지를 측정한다. 속도 추정 오차, 궤적 예측 정확도, 오검출률(False Positive Rate), 미검출률(Missed Detection Rate), 처리 지연 시간(Latency), 처리 속도(Throughput), 그리고 다양한 환경 변화에 대한 강인성(Robustness)이 실제 시스템 성능을 결정하는 핵심 평가 요소이다.



현장 검증(Field Validation)은 실제 산업 환경과 유사한 조건에서 수행되어야 한다. 여러 작업자와 차량이 동시에 이동하는 환경, 복잡한 교차로, 다양한 조명 조건, 악천후, 예측하기 어려운 사람의 행동 등을 포함하여 시험해야 한다. 긴급 제동 성능, 회피 성공률, 최소 안전 거리 유지, 일시적인 가림 이후의 복구 능력, 장시간 추적 안정성 등을 종합적으로 평가해야 한다. 또한 모든 데이터를 지속적으로 기록하여 오프라인 분석을 수행하면 실패 원인을 재현하고 인지 알고리즘을 반복적으로 개선할 수 있다.



미래의 동적 장애물 감지 시스템은 파운데이션 모델(Foundation Model), 멀티모달 인지(Multimodal Perception), 자기지도학습(Self-Supervised Learning), 그리고 실시간 월드 모델(World Model)을 적극적으로 활용하게 될 것이다. 미래의 로봇은 단순히 현재 움직임에 반응하는 수준을 넘어 사람의 의도를 예측하고, 다른 자율 시스템과 자연스럽게 협력하며, 실제 운용 과정에서 지속적으로 자신의 인지 모델을 개선하게 될 것이다. 이러한 발전은 복잡하고 끊임없이 변화하는 산업, 상업, 실외 환경에서 더욱 안전하고 부드러우며 효율적인 자율주행을 실현하는 핵심 기술이 될 것이다.



## 19.4 Small Object Detection

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

업로드된 자료는 **"19_04_Small_Object_Detection"**이 **Volume 03 → 19_Obstacle_Detection**의 하위 항목이라는 문서 구조를 제공하며, 본문 내용은 포함하고 있지 않습니다. 아래 내용은 업로드된 구조를 기반으로 해당 장에 적합하도록 확장한 번역본입니다.



소형 물체 감지(소형 물체 감지, Small Object Detection)는 센서 데이터에서 매우 작은 영역만을 차지하지만 자율주행 모바일 로봇(Autonomous Mobile Robot, AMR)의 안전에 중대한 영향을 미칠 수 있는 장애물을 식별하는 특수한 인지(Perception) 기술이다. 차량, 벽, 보행자와 같은 큰 물체는 센서에서 풍부한 특징 정보를 생성하지만, 작은 물체는 포인트 클라우드(Point Cloud), 영상 픽셀, 또는 레이더 반사 신호가 매우 제한적으로 나타난다. 그럼에도 불구하고 이러한 물체는 로봇과의 충돌, 장비 손상, 작업 중단, 산업 현장에서의 안전사고를 유발할 수 있다. 따라서 소형 물체 감지는 기존 장애물 감지 기능을 보완하는 수준이 아니라 안전한 자율주행을 위한 필수 요소로 간주된다.



소형 장애물은 운용 환경에 따라 매우 다양한 형태로 존재한다. 실내 환경에서는 볼트, 너트, 공구, 전선, 호스, 팔레트(Pallet)의 돌출부, 떨어진 박스, 플라스틱 용기, 전기 커넥터, 각종 잔해, 소형 유지보수 장비 등이 대표적인 예이다. 실외에서는 돌, 나뭇가지, 건설 자재, 교통 콘(Traffic Cone), 연석(Curb), 포트홀(Pothole), 자갈, 파이프, 손상된 도로 구조물 등이 주요 대상이 된다. 이러한 물체는 크기는 작지만 바퀴, 서스펜션(Suspension), 센서, 견인 장치, 적재물의 안정성 등에 영향을 주어 로봇의 정상적인 운행을 방해할 수 있다.



소형 물체는 센서에서 생성되는 정보가 매우 제한적이기 때문에 대형 장애물보다 감지가 훨씬 어렵다. 작은 물체는 고해상도 카메라 영상에서도 몇 개의 픽셀만 차지하거나, LiDAR에서는 소수의 포인트만 생성하는 경우가 많다. 또한 물체와의 거리가 멀어질수록 센서가 획득하는 특징 정보는 더욱 감소하므로 안정적인 인식이 어려워진다. 따라서 인지 시스템은 공간 해상도(Spatial Resolution)와 감지 민감도를 최대한 높이는 동시에 오검출(False Positive)을 최소화하도록 설계되어야 한다.



센서 해상도(Sensor Resolution)는 소형 물체 인지 성능을 결정하는 핵심 요소이다. 고해상도 RGB 카메라는 색상, 질감, 형태와 같은 세밀한 특징을 이용하여 작은 물체를 인식할 수 있도록 지원한다. 채널 수가 많은 3차원 LiDAR는 더욱 촘촘한 포인트 클라우드를 생성하여 작은 물체의 기하학적 구조를 보다 정확하게 표현한다. 근거리 깊이 카메라(Depth Camera)는 주변 장애물의 정밀한 형상을 제공하며, 밀리미터파 레이더(Millimeter-Wave Radar)는 악천후에서도 금속 물체를 안정적으로 감지하는 데 도움을 준다. 따라서 적절한 해상도와 감지 범위를 갖는 센서를 선택하는 것은 시스템 설계에서 가장 중요한 결정 중 하나이다.



센서의 설치 위치(Sensor Placement)는 소형 물체의 가시성에 큰 영향을 미친다. 카메라나 LiDAR를 너무 높은 위치에 설치하면 낮은 높이의 장애물이 얕은 시야각 때문에 보이지 않거나 로봇 차체에 의해 가려질 수 있다. 반대로 센서를 지나치게 낮게 설치하면 먼지, 물, 진동, 충격 등에 쉽게 노출되어 신뢰성이 저하될 수 있다. 실제 산업용 로봇은 원거리 감지를 위한 상부 센서와 근거리 지면 감지를 위한 하부 센서를 함께 사용하여 다양한 높이의 장애물을 안정적으로 인식하는 구조를 채택하는 경우가 많다.



지면 분할(지면 분할, Ground Segmentation)은 대부분의 소형 물체 감지 알고리즘에서 가장 기본적인 처리 단계이다. 인지 시스템은 평면 추정(Plane Fitting), 고도 지도(Elevation Map), 복셀 맵(Voxel Map) 등 다양한 기법을 이용하여 먼저 지면을 추정한다. 이후 지면보다 일정 높이 이상 돌출된 포인트나 픽셀을 후보 장애물로 분류한다. 특히 울퉁불퉁한 노면, 경사로, 비포장 도로에서는 정확한 지면 추정이 이루어지지 않으면 정상적인 지형이 장애물로 잘못 인식될 수 있으므로 매우 높은 정확도가 요구된다.



영상 기반(Image-Based) 소형 물체 감지는 현대 딥러닝(Deep Learning) 구조를 적극 활용한다. 특징 피라미드 네트워크(Feature Pyramid Network), 다중 해상도 합성곱 구조(Multi-Scale Convolution), 어텐션 메커니즘(Attention Mechanism), 트랜스포머(Transformer) 기반 검출 모델은 높은 해상도의 특징 정보를 유지하면서 동시에 넓은 장면의 의미 정보를 함께 활용한다. 이러한 다중 스케일(Multi-Scale) 학습 구조는 작은 잔해부터 대형 산업 차량까지 하나의 통합된 인지 시스템에서 동시에 검출할 수 있도록 지원한다.



LiDAR 기반 소형 물체 감지는 희소한 포인트 클라우드에서 작은 기하학적 구조를 추출하는 데 중점을 둔다. 클러스터링(Clustering) 알고리즘은 인접한 포인트를 하나의 후보 물체로 그룹화하며, 복셀 표현(Voxel Representation)은 불균일한 포인트 밀도를 정규화하여 3차원 신경망이 안정적으로 처리할 수 있도록 한다. 작은 물체는 생성하는 포인트 수가 매우 적기 때문에 노이즈 제거, 이상치 제거, 포인트 보강(Point Cloud Densification), 시간적 누적(Temporal Accumulation)과 같은 전처리 과정이 감지 성능 향상에 매우 중요한 역할을 수행한다.



시간적 통합(Temporal Integration)은 소형 물체 감지 성능을 크게 향상시키는 기술이다. 시스템은 단일 센서 프레임만 사용하는 것이 아니라 여러 개의 연속된 프레임에서 얻어진 정보를 함께 결합한다. 로봇이 이동하면서 동일한 물체를 다양한 시점에서 관찰하게 되면 이전에는 불확실했던 물체에 대한 정보가 점차 누적된다. 이러한 시간적 누적은 감지 신뢰도를 향상시키고, 단순한 센서 노이즈를 실제 장애물로 잘못 인식할 가능성을 줄여준다.



센서 융합(센서 융합, Sensor Fusion)은 어느 하나의 센서만으로는 해결할 수 없는 한계를 보완한다. 카메라는 풍부한 의미 정보(Semantic Information)를 제공하고, LiDAR는 정확한 3차원 형상을 제공하며, 깊이 카메라는 근거리 공간 정보를 보완한다. 레이더는 비나 안개 환경에서도 안정적으로 동작하고, 초음파 센서(Ultrasonic Sensor)는 매우 가까운 사각지대를 보호한다. 이러한 다양한 센서의 정보를 융합하면 소형 장애물에 대한 더욱 완전한 환경 표현을 생성할 수 있으며 센서 고유의 한계로 인한 불확실성도 감소시킬 수 있다.



소형 물체 감지를 위한 인공지능(AI) 모델은 작은 물체가 충분히 포함된 데이터셋(Dataset)을 기반으로 학습되어야 한다. 데이터 수집 과정에서는 다양한 크기, 재질, 색상, 조명 조건, 배경 환경, 기상 조건, 센서 시점을 모두 포함해야 한다. 데이터셋이 대형 물체 중심으로 구성되면 모델은 작은 물체를 충분히 학습하지 못하게 된다. 따라서 크기 변화(Scaling), 자르기(Cropping), 회전(Rotation), 밝기 변화(Brightness Adjustment), 합성 데이터(Synthetic Data), 도메인 랜덤화(Domain Randomization)와 같은 데이터 증강(Data Augmentation) 기법이 일반적으로 활용된다.



거리(Distance)에 따른 감지 성능 변화도 반드시 고려해야 한다. 로봇 가까이에 있는 작은 물체는 더 많은 픽셀과 포인트를 생성하므로 비교적 쉽게 검출할 수 있다. 그러나 거리가 증가할수록 물체의 크기가 센서 데이터에서 급격히 감소하므로 검출 정확도 역시 함께 감소한다. 따라서 최대 주행 속도에서의 제동 거리보다 충분히 긴 거리에서 소형 장애물을 감지할 수 있도록 시스템을 설계해야 실제 운용 환경에서 안전성을 확보할 수 있다.



환경 조건(Environmental Conditions)은 소형 물체 감지를 더욱 어렵게 만든다. 그림자, 반사되는 바닥, 젖은 노면, 눈, 진흙, 낙엽, 먼지, 식생, 변화하는 조명 조건은 작은 장애물을 부분적으로 가리거나 허위 패턴을 생성할 수 있다. 실외 환경에서는 비, 안개, 직사광선, 강풍에 의해 이동하는 잔해 등이 추가적인 불확실성을 유발한다. 따라서 실제 시스템은 적응형 전처리(Adaptive Preprocessing), 센서 융합, 신뢰도 추정(Confidence Estimation)을 함께 적용하여 다양한 환경 변화 속에서도 안정적인 감지 성능을 유지하도록 설계된다.



오검출 억제(False Positive Suppression)는 특히 중요하다. 바닥의 질감 변화, 페인트 표시선, 물웅덩이, 배수구, 센서 잡음, 순간적인 조명 변화 등이 실제 장애물처럼 보일 수 있기 때문이다. 오검출이 지나치게 많으면 로봇은 불필요하게 감속하거나 반복적으로 경로를 재계획하게 되어 전체 작업 효율이 크게 저하된다. 신뢰도 평가, 시간적 일관성 분석(Temporal Consistency Analysis), 의미 기반 분류(Semantic Classification), 센서 간 교차 검증(Cross-Sensor Verification)은 실제 장애물과 환경 노이즈를 효과적으로 구분하는 데 사용된다.



소형 물체 감지는 지역 경로 계획(Local Path Planning)과 차량 제어(Motion Control)에 직접적인 영향을 미친다. 장애물이 확인되면 내비게이션 시스템은 해당 물체를 안전하게 넘어갈 수 있는지, 우회해야 하는지, 속도를 줄여야 하는지, 또는 완전히 정지해야 하는지를 판단한다. 이러한 결정은 로봇의 지상고(Ground Clearance), 바퀴 크기, 서스펜션 특성, 적재물 안정성, 노면 마찰력, 안전 정책 등을 종합적으로 고려하여 이루어진다. 따라서 단순한 감지뿐 아니라 장애물이 실제 차량에 미치는 영향을 이해하는 것이 지능형 자율주행의 핵심 요소이다.



기능 안전(Functional Safety) 측면에서는 로봇의 안정성과 작업자의 안전을 위협할 수 있는 소형 장애물을 확실하게 감지해야 한다. 일부 산업 환경에서는 인증된 안전 센서(Safety Sensor)가 미리 정의된 보호 구역 내에서 장애물을 반드시 검출해야 한다. AI 기반 인지 시스템은 이러한 인증 시스템을 대체하는 것이 아니라 다양한 종류의 물체를 인식하고 더욱 풍부한 의미 정보를 제공하는 역할을 수행한다. 이처럼 결정론적인 안전 시스템과 지능형 인지 시스템을 함께 사용하는 계층형 보호 구조(Layered Protection Architecture)가 산업용 자율주행 로봇에서 일반적으로 사용된다.



실시간 구현(Real-Time Implementation)을 위해서는 높은 해상도의 센서 데이터와 복잡한 신경망을 효율적으로 처리할 수 있도록 시스템을 최적화해야 한다. 엣지 AI 가속기(Edge AI Accelerator), GPU 기반 추론(Inference), TensorRT 최적화, 양자화 신경망(Quantized Neural Network), 비동기 인지 파이프라인(Asynchronous Perception Pipeline), 관심 영역 처리(Region of Interest Processing)는 지연 시간을 줄이면서도 높은 검출 성능을 유지하도록 지원한다. 효율적인 소프트웨어 구조는 안전한 자율주행을 위한 충분한 업데이트 주기를 지속적으로 유지하는 데 필수적이다.



소형 물체 감지의 성능 평가는 일반적인 객체 검출(Object Detection) 성능만으로는 충분하지 않다. 최소 검출 가능 크기(Minimum Detectable Object Size), 검출 거리, 위치 정확도(Localizaton Accuracy), 오검출률(False Positive Rate), 미검출률(Missed Detection Rate), 추론 지연 시간(Inference Latency), 환경 변화에 대한 강인성(Robustness), 장기간 운용 신뢰성(Long-Term Reliability) 등을 함께 평가해야 한다. 또한 다양한 재질, 불규칙한 형상, 서로 다른 반사율, 다양한 높이를 가진 물체를 포함한 시험을 수행하여 실제 운용 환경에서의 성능을 종합적으로 검증해야 한다.



현장 검증(Field Validation)은 상용 배치 이전의 마지막 개발 단계이다. 실제 산업 환경에는 공구, 케이블, 포장재, 건설 잔해, 울퉁불퉁한 노면, 손상된 바닥 등 다양한 소형 장애물이 존재하므로 이러한 조건을 충분히 포함하여 시험해야 한다. 모든 센서 데이터를 지속적으로 기록하면 오프라인 실패 분석(Offline Failure Analysis), 알고리즘 개선, 실제 운용 중 수집된 어려운 사례를 이용한 재학습(Retraining)이 가능해진다. 이러한 반복적인 개선 과정은 시스템의 강인성을 높이고 상용 운용 이후 발생할 수 있는 예기치 않은 오류를 지속적으로 감소시킨다.



미래의 소형 물체 감지 시스템은 파운데이션 모델(Foundation Model), 멀티모달 인지(Multimodal Perception), 자기지도학습(Self-Supervised Learning), 고해상도 3차원 월드 모델(3D World Model), 적응형 센서 융합(Adaptive Sensor Fusion)을 적극적으로 활용하게 될 것이다. 미래의 자율주행 로봇은 단순히 작은 물체를 발견하는 수준을 넘어 해당 물체가 통과 가능한지(Traversability), 얼마나 위험한지, 어떤 특성을 가지는지를 스스로 추론하고 실제 운용 경험을 통해 지속적으로 인지 능력을 향상시키게 될 것이다. 이러한 기술은 산업 현장, 물류센터, 건설 현장, 실외 자율주행 환경에서 더욱 안전하고 효율적이며 지능적인 자율주행을 가능하게 하는 핵심 기반 기술이 될 것이다.



## 19.5 Hanging and Overhanging Obstacle Detection

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

매달린 장애물 및 돌출 장애물 감지(매달린 장애물 및 돌출 장애물 감지, Hanging and Overhanging Obstacle Detection)는 지면 위에 위치하지만 자율주행 모바일 로봇(Autonomous Mobile Robot, AMR)의 충돌 가능 영역(Collision Envelope) 안에 존재하는 장애물을 인식하는 특수한 인지(Perception) 기술이다. 기존의 장애물 감지가 주행면(Driving Surface)에 놓인 물체를 중심으로 수행되는 것과 달리, 이 기술은 로봇의 센서, 적재물, 로봇 암(Robot Arm), 마스트(Mast), 안테나, 또는 높은 위치에 장착된 장비와 충돌할 수 있는 3차원 구조물을 고려한다. 이러한 장애물을 정확하게 감지하는 것은 창고, 공장, 병원, 건설 현장, 실외 물류 환경 등에서 천장 구조물과 상부 시설물이 빈번하게 존재하는 환경에서 충돌을 방지하기 위해 매우 중요하다.



매달린 장애물(Hanging Obstacle)은 천장, 건물 구조물, 산업 장비 또는 임시 지지대에 매달려 있는 물체를 의미한다. 대표적인 예로 전기 케이블, 공기 호스, 체인, 경고 표지판, 매달린 공구, 조명 장치, 보호 커튼, 임시 안전 차단막, 실외 환경의 낮게 늘어진 나뭇가지 등이 있다. 반면 돌출 장애물(Overhanging Obstacle)은 지면에 닿아 있지는 않지만 로봇의 이동 경로 위로 수평 방향으로 돌출된 구조물을 의미하며, 컨베이어 시스템, 선반, 적재 플랫폼, 발코니, 지붕 처마, 배관망, 교량, 나뭇가지, 건물의 돌출 구조 등이 이에 해당한다. 이러한 구조물은 주행면을 점유하지 않더라도 로봇의 3차원 운행 공간과 충돌할 수 있다.



기존의 2차원 장애물 감지 시스템은 매달린 장애물을 인식하지 못하는 경우가 많다. 범퍼 높이에 설치된 2차원 LiDAR는 바닥에 있는 장애물은 안정적으로 감지할 수 있지만, 스캔 평면보다 높은 위치에 있는 물체는 완전히 놓칠 수 있다. 또한 센서 타워(Sensor Tower), 로봇 암, 카메라, 통신 안테나, 높은 적재물 등을 장착하면서 로봇의 전체 높이가 증가하면 상부 구조물과의 충돌 위험도 크게 증가한다. 따라서 3차원 인지(3D Perception)는 선택적인 기능이 아니라 필수적인 안전 기술이 된다.



로봇의 물리적 크기(Physical Dimensions)는 매달린 장애물 감지 요구사항을 결정하는 중요한 요소이다. 인지 시스템은 단순히 바닥에서 차지하는 면적(Footprint)만이 아니라 로봇의 전체 3차원 충돌 영역을 이해해야 한다. 여기에는 로봇 높이, 적재물 크기, 마스트의 상승 높이, 로봇 암의 작업 공간, 센서의 장착 위치, 가속이나 서스펜션 움직임에 따른 동적 변화까지 포함된다. 안전한 자율주행은 주변 환경과 이러한 동적인 로봇 형상을 지속적으로 비교하여 이루어진다.



3차원 LiDAR는 매달린 장애물과 돌출 장애물을 감지하는 가장 효과적인 센서 중 하나이다. 여러 개의 수직 스캔 채널이 서로 다른 높이에서 밀집된 포인트 클라우드(Point Cloud)를 생성하여 상부 구조물을 정확하게 측정할 수 있다. 최신 인지 알고리즘은 이러한 포인트 클라우드를 지면, 수직 구조물, 매달린 물체, 돌출 구조물로 분리하여 분석한다. 또한 높이 정보를 이용하여 좁은 통로나 높은 구조물 아래를 통과하기 전에 충분한 여유 공간(Clearance)이 존재하는지 판단할 수 있다.



깊이 카메라(Depth Camera)는 LiDAR를 보완하는 중요한 역할을 수행한다. 스테레오 카메라(Stereo Camera), 구조광 카메라(Structured Light Camera), ToF(Time-of-Flight) 카메라는 근거리 및 중거리에서 조밀한 3차원 깊이 정보를 제공하여 LiDAR에서 충분한 포인트를 생성하지 못하는 상부 장애물을 보완한다. 또한 카메라는 딥러닝 기반 객체 인식을 통해 케이블, 표지판, 나뭇가지, 산업 장비 등을 의미적으로 분류할 수 있다. 이러한 기하학 정보와 의미 정보의 결합은 장애물 감지 정확도와 분류 성능을 동시에 향상시킨다.



다중 카메라 시스템(Multi-Camera System)은 상부 장애물 인지 범위를 더욱 확대한다. 위쪽을 향한 카메라는 천장 구조물과 매달린 설비를 관찰하며, 전방 카메라는 로봇이 접근하기 전에 돌출 구조물을 미리 감지한다. 광각 렌즈(Wide-Angle Lens)는 넓은 시야를 제공하지만 광학 왜곡이 발생하므로 정밀한 보정(Calibration)이 필요하다. 적절한 카메라 배치는 지면과 상부 장애물을 동시에 관찰하면서도 사각지대(Blind Zone)를 최소화할 수 있도록 설계되어야 한다.



포인트 클라우드 처리(Point Cloud Processing)는 매달린 구조물을 식별하는 핵심 기술이다. 먼저 지면 분할(Ground Segmentation)을 수행하여 바닥 포인트를 제거한 후 높은 위치에 존재하는 포인트 클러스터를 독립적으로 분석한다. 높이 임계값(Height Threshold), 연결 요소 클러스터링(Connected Component Clustering), 복셀 분석(Voxel Analysis), 의미 분할(Semantic Segmentation) 등을 이용하여 매달린 물체를 벽, 천장, 영구 구조물과 구분한다. 최종적으로 생성된 장애물 정보에는 3차원 위치, 크기, 통과 가능 높이(Clearance Height), 신뢰도, 의미 분류 정보 등이 포함되어 내비게이션 시스템으로 전달된다.



높이 추정(Height Estimation)은 매달린 장애물 감지에서 가장 중요한 출력 정보 가운데 하나이다. 시스템은 단순히 장애물의 존재 여부만 판단하는 것이 아니라 장애물과 지면 사이의 실제 높이를 계산한다. 이후 이 높이를 로봇의 현재 높이와 예상 이동 궤적을 포함한 충돌 영역과 지속적으로 비교한다. 수 센티미터 수준의 작은 오차도 고가의 센서나 적재물과 충돌할 수 있으므로 매우 높은 수직 위치 정확도(Vertical Localization Accuracy)가 요구된다.



의미 분류(Semantic Classification)는 상부 장애물의 종류를 구분하여 보다 지능적인 의사결정을 가능하게 한다. 임시로 매달린 케이블은 움직임을 예측하기 어려우므로 우회하는 것이 바람직하지만, 건물의 고정 구조물은 지도(Map)에 안전하게 등록하여 지속적으로 활용할 수 있다. 비닐 커튼이나 가벼운 식생과 같은 유연한 물체는 제한적인 접촉이 가능할 수도 있지만, 강철 빔이나 콘크리트 구조물은 반드시 완전히 회피해야 한다. 따라서 의미 기반 이해는 안전성과 작업 효율을 동시에 향상시키는 중요한 요소이다.



센서 융합(센서 융합, Sensor Fusion)은 상부 장애물 감지의 신뢰성을 크게 향상시킨다. 3차원 LiDAR는 정확한 기하학 정보를 제공하고, 카메라는 객체 종류를 인식하며, 레이더(Radar)는 악천후에서도 대형 금속 구조물을 안정적으로 탐지한다. IMU(Inertial Measurement Unit)는 로봇이 움직이는 동안 센서 자세를 안정화하여 인지 정확도를 높인다. 다양한 센서를 융합하면 부분적인 가림(Occlusion), 조명 변화, 반사 표면, 희소한 포인트 클라우드 등으로 인해 발생하는 불확실성을 효과적으로 줄일 수 있다.



움직이는 매달린 장애물(Dynamic Hanging Obstacle)은 추가적인 어려움을 발생시킨다. 매달린 케이블은 바람에 흔들릴 수 있고, 크레인은 작업 중 적재물을 이동시키며, 공기 호스는 장비의 움직임에 따라 흔들릴 수 있다. 또한 나뭇가지는 기상 변화에 따라 지속적으로 움직인다. 따라서 동적 환경에서는 단순한 장애물 감지뿐 아니라 객체 추적(Object Tracking), 속도 추정(Velocity Estimation), 움직임 예측(Motion Prediction)을 함께 수행하여 미래의 충돌 가능성을 평가해야 한다.



환경 조건(Environmental Conditions)은 인지 성능에 큰 영향을 미친다. 조도가 낮으면 카메라 성능이 저하되며, 비, 안개, 눈, 먼지는 광학 센서와 LiDAR 모두의 성능을 감소시킨다. 반사가 심한 금속 배관은 LiDAR 측정값을 불안정하게 만들 수 있으며, 유리나 투명 플라스틱 커튼은 일부 센서에서 감지가 어려울 수 있다. 강인한 인지 시스템은 적응형 센서 융합과 신뢰도 평가를 통해 이러한 환경 변화에 대응한다.



구조물의 여유 높이(Clearance) 정보를 포함하는 디지털 지도(Digital Map)는 내비게이션 신뢰성을 더욱 향상시킨다. 창고와 공장은 대부분 상부 구조물이 고정되어 있으므로 고해상도 3차원 지도에는 교량 높이, 컨베이어 높이, 출입문 크기, 배관망, 천장 구조 등이 저장된다. 실제 운행 중에는 센서가 지도 정보를 지속적으로 검증하면서 동시에 지도에 존재하지 않는 임시 매달린 장애물도 함께 탐지한다.



내비게이션 시스템(Navigation System)은 상부 장애물 정보를 이용하여 현재 계획된 경로가 안전한지를 판단한다. 터널, 컨베이어 아래, 출입문, 적재 플랫폼 등을 통과하기 전에 충분한 여유 공간이 존재하는지를 계산한다. 공간이 부족하면 다른 경로를 선택하거나, 작업자의 개입을 요청하거나, 조절 가능한 장비를 낮추거나, 위험 구역 진입 전에 정지한다. 이러한 여유 공간 기반 경로 계획(Clearance-Aware Planning)은 상부 충돌 위험을 크게 감소시킨다.



기능 안전(Functional Safety) 측면에서는 상부 충돌로부터 로봇을 보호하는 것이 매우 중요하다. 상부 충돌은 차체보다 높은 위치에 장착된 고가의 센서, 통신 장비, 로봇 암, 적재물 등을 손상시키는 경우가 많다. 따라서 일부 안전 시스템은 바닥뿐 아니라 로봇 상부까지 포함하는 3차원 보호 영역(Protective Volume)을 정의한다. AI 기반 인지는 이러한 결정론적(Deterministic) 안전 시스템을 보완하여 다양한 장애물 종류를 인식하고 보다 풍부한 환경 정보를 제공한다.



실시간 구현(Real-Time Implementation)을 위해서는 대용량의 3차원 센서 데이터를 효율적으로 처리해야 한다. GPU 가속(GPU Acceleration), 복셀 압축(Voxel Compression), 관심 영역 처리(Region of Interest Processing), 비동기 인지 파이프라인(Asynchronous Perception Pipeline), 최적화된 딥러닝 추론은 계산 지연을 줄이면서도 높은 감지 성능을 유지한다. 실제 시스템은 계산량과 업데이트 주기(Update Frequency)를 균형 있게 조정하여 연속적인 자율주행 중에도 안전한 충돌 회피를 보장한다.



매달린 장애물 및 돌출 장애물 감지의 성능 평가는 수직 감지 거리(Vertical Detection Range), 최소 감지 가능 크기(Minimum Detectable Obstacle Size), 여유 높이 계산 정확도(Clearance Estimation Accuracy), 의미 분류 성능, 오검출률(False Positive Rate), 미검출률(Missed Detection Rate), 처리 지연 시간(Processing Latency), 환경 변화에 대한 강인성(Robustness) 등을 포함한다. 또한 다양한 재질, 형상, 설치 방식, 높이를 가진 장애물을 이용하여 조명 변화, 기상 조건, 움직임 변화에 대한 종합적인 성능 평가가 수행되어야 한다.



현장 검증(Field Validation)은 실제 산업 환경을 최대한 반영하여 수행되어야 한다. 배관, 케이블, 컨베이어, 매달린 공구, 건설 장비, 창고 선반, 적재 플랫폼, 나뭇가지, 교량 등 다양한 상부 구조물을 포함한 시험 환경이 필요하다. 지속적인 센서 데이터 기록은 실패 원인 재현, 알고리즘 개선, 센서 보정, 데이터셋 확장에 활용되며 실제 운용 과정에서 수집된 경험을 기반으로 시스템을 지속적으로 발전시킬 수 있다.



미래의 매달린 장애물 및 돌출 장애물 감지 시스템은 파운데이션 모델(Foundation Model), 멀티모달 3차원 장면 이해(Multimodal 3D Scene Understanding), 자기지도학습(Self-Supervised Learning), 의미 기반 월드 모델(Semantic World Model), 예측 기반 환경 추론(Predictive Environmental Reasoning)을 적극적으로 활용하게 될 것이다. 미래의 자율주행 로봇은 단순히 상부 장애물을 발견하는 수준을 넘어 구조물의 목적과 특성을 이해하고, 통과 가능성(Traversability)을 평가하며, 장애물의 움직임을 예측하고, 장기적인 충돌 위험을 분석하며, 실제 운용 경험을 통해 지속적으로 인지 능력을 향상시키게 될 것이다. 이러한 기술은 더욱 복잡한 산업, 상업, 물류, 실외 환경에서 안전하고 지능적이며 신뢰성 높은 자율주행을 실현하는 핵심 기반 기술이 될 것이다.



## 19.6 Multi-Sensor Obstacle Detection

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

다중 센서 장애물 감지(다중 센서 장애물 감지, Multi-Sensor Obstacle Detection)는 여러 종류의 센서에서 획득한 정보를 통합하여 단일 센서만으로는 얻을 수 없는 높은 신뢰성, 정확성, 강인성을 확보하는 고급 인지(Perception) 기술이다. 각각의 센서는 고유한 장점과 한계를 가지고 있기 때문에 하나의 센서만으로 모든 환경에서 안정적인 장애물 감지를 수행하는 것은 현실적으로 어렵다. LiDAR, 카메라(Camera), 레이더(Radar), 초음파 센서(Ultrasonic Sensor), 깊이 카메라(Depth Camera), GNSS(Global Navigation Satellite System), IMU(Inertial Measurement Unit), 휠 오도메트리(Wheel Odometry)의 정보를 함께 활용함으로써 자율주행 모바일 로봇(Autonomous Mobile Robot, AMR)은 주변 환경을 더욱 완전하게 이해하고 다양한 운용 환경에서 안전한 자율주행을 수행할 수 있다.



단일 센서 기반 인지 시스템은 이상적인 환경을 벗어나면 성능이 급격히 저하되는 경우가 많다. 카메라는 풍부한 의미 정보(Semantic Information)를 제공하지만 어두운 환경, 강한 역광, 안개, 폭우에서는 성능이 크게 감소한다. LiDAR는 정확한 3차원 기하학 정보를 제공하지만 먼 거리에서는 포인트 클라우드(Point Cloud)가 희소해지고, 투명하거나 강한 반사 특성을 가진 물체에서는 정확한 측정이 어려울 수 있다. 레이더는 악천후에서도 안정적으로 동작하며 직접적인 속도 정보를 제공하지만 공간 해상도가 낮다. 초음파 센서는 근거리 감지에는 매우 효과적이지만 장거리 인지에는 적합하지 않다. 다중 센서 구조는 이러한 개별 센서의 한계를 상호 보완함으로써 전체 시스템의 신뢰성을 향상시킨다.



다중 센서 장애물 감지의 궁극적인 목적은 단순히 여러 센서의 결과를 결합하는 것이 아니라 더욱 높은 신뢰도와 낮은 불확실성을 갖는 통합 환경 표현(Environment Representation)을 생성하는 것이다. 인지 시스템은 각 센서를 독립적으로 처리하는 대신 여러 센서의 관측 결과가 서로 일치하는지 또는 불일치하는지를 지속적으로 평가하며, 각 측정값의 신뢰도를 함께 추정한다. 이러한 확률 기반 추론(Probabilistic Reasoning)은 일부 센서가 일시적으로 성능이 저하되거나 부분적인 고장이 발생하더라도 안정적인 장애물 감지를 가능하게 한다.



센서 다양성(Sensor Diversity)은 다중 센서 인지의 가장 큰 장점 가운데 하나이다. LiDAR와 깊이 카메라는 물체의 기하학적 형태와 위치를 제공하고, RGB 카메라는 사람, 지게차, 차량, 팔레트, 교통 표지판 등의 의미 정보를 제공한다. 레이더는 상대 속도와 장거리 물체를 안정적으로 측정하며, IMU는 차량의 움직임을 보정하여 인지 결과를 안정화한다. GNSS와 오도메트리(Odometry)는 정확한 위치 정보를 제공하여 시간에 따라 일관된 환경 표현을 유지할 수 있도록 지원한다.



성공적인 센서 융합(Sensor Fusion)은 정확한 센서 보정(Calibration)에서 시작된다. 내부 보정(Intrinsic Calibration)은 개별 센서의 내부 파라미터를 추정하며, 외부 보정(Extrinsic Calibration)은 서로 다른 센서 사이의 정확한 위치와 자세 관계를 계산한다. 작은 보정 오차만 발생해도 동일한 물체가 서로 다른 위치에 나타나거나, 동일 장애물이 여러 개로 인식되거나, 환경 모델이 일관성을 잃을 수 있다. 따라서 장기간 운용에서도 센서 보정 정확도를 유지하는 것은 매우 중요한 엔지니어링 과제이다.



시간 동기화(Time Synchronization) 역시 다중 센서 장애물 감지에서 필수적인 요소이다. 서로 다른 센서에서 측정된 데이터를 동일한 시점의 정보로 정렬해야 정확한 융합이 가능하다. 이를 위해 하드웨어 트리거(Hardware Trigger), PTP(Precision Time Protocol), NTP(Network Time Protocol), 동기화된 타임스탬프(Time Stamp) 등이 사용된다. 시간 동기화가 정확하지 않으면 이동하는 장애물이 서로 다른 위치에 존재하는 것처럼 보이게 되어 속도 추정과 객체 추적(Object Tracking)의 정확도가 크게 저하된다.



센서 융합 구조(Sensor Fusion Architecture)는 일반적으로 초기 융합(Early Fusion), 중간 융합(Mid-Level Fusion), 후기 융합(Late Fusion)으로 구분된다. 초기 융합은 원시 센서 데이터를 직접 결합하여 높은 정보 활용도를 제공하지만 계산량이 매우 크다. 중간 융합은 각 센서에서 추출된 특징 정보를 결합하는 방식이며, 후기 융합은 각 센서가 독립적으로 생성한 객체 검출 결과를 확률적으로 통합하는 방법이다. 후기 융합은 모듈화가 용이하여 유지보수와 시스템 확장에 유리하다.



LiDAR-카메라 융합(LiDAR-Camera Fusion)은 가장 널리 사용되는 다중 센서 장애물 감지 방식이다. LiDAR는 정확한 3차원 좌표를 제공하고, 카메라는 딥러닝 기반 객체 인식을 통해 의미 정보를 제공한다. 포인트 클라우드를 영상 좌표계에 투영(Project)하면 하나의 객체는 위치와 크기뿐 아니라 객체 종류와 시각적 특징까지 함께 가지게 된다. 결과적으로 장애물은 객체 종류(Class), 위치(Position), 자세(Orientation), 크기(Dimensions), 신뢰도(Confidence) 등을 모두 포함하는 풍부한 표현으로 변환된다.



레이더-카메라 융합(Radar-Camera Fusion)은 악천후와 고속 실외 자율주행에서 특히 효과적이다. 레이더는 도플러(Doppler) 정보를 이용하여 물체의 상대 속도를 정확하게 측정하며, 카메라는 물체의 종류와 시각적 정보를 제공한다. 레이더는 잠재적인 장애물의 위치를 카메라에 알려주고, 카메라는 레이더가 검출한 물체의 경계와 종류를 더욱 정확하게 분석한다. 이러한 상호 보완 관계는 비, 안개, 눈, 야간 환경에서도 높은 인지 성능을 유지할 수 있도록 지원한다.



LiDAR-레이더 융합(LiDAR-Radar Fusion)은 정확한 공간 측정과 장거리 이동 물체 감지를 동시에 수행한다. 레이더는 악천후에서도 먼 거리의 차량이나 대형 금속 구조물을 안정적으로 감지하며, LiDAR는 그 물체의 정확한 형상과 위치를 계산한다. 두 센서를 함께 사용하면 어느 하나의 센서만 사용할 때보다 훨씬 우수한 장애물 감지 성능을 얻을 수 있다.



깊이 카메라(Depth Camera)는 근거리 장애물 감지를 더욱 향상시킨다. 창고 선반, 작업 공간, 도킹 스테이션(Docking Station), 작업자 주변과 같이 근거리에서 복잡한 구조를 가지는 환경에서는 조밀한 깊이 정보(Dense Depth Map)가 LiDAR를 효과적으로 보완한다. RGB 영상과 깊이 정보를 함께 활용하면 복잡한 구조를 가진 장애물을 더욱 정확하게 분리하고 인식할 수 있다.



초음파 센서(Ultrasonic Sensor)는 매우 가까운 거리에서 발생하는 사각지대를 보호하는 역할을 한다. 도킹(Docking), 주차(Parking), 팔레트 적재(Pallet Handling), 좁은 통로 주행과 같은 상황에서는 수 센티미터 거리의 장애물도 정확하게 감지해야 한다. 초음파 센서는 장거리 센서가 관찰하지 못하는 근거리 영역을 보완하여 전체 운용 거리에서 연속적인 장애물 감지를 가능하게 한다.



객체 연관(Object Association)은 다중 센서 장애물 감지에서 매우 중요한 단계이다. 서로 다른 센서에서 검출된 객체가 실제로 동일한 물체인지 판단해야 하기 때문이다. 데이터 연관(Data Association) 알고리즘은 위치, 크기, 이동 특성, 의미 정보, 시간적 일관성 등을 종합적으로 비교하여 동일 객체 여부를 판단한다. 정확한 객체 연관은 동일 장애물이 여러 번 생성되는 문제를 방지하고 여러 센서의 정보를 통합하여 더욱 높은 신뢰도를 제공한다.



신뢰도 추정(Confidence Estimation)은 센서별 가중치를 동적으로 조절하는 중요한 기능이다. 모든 센서를 동일하게 취급하는 것이 아니라 환경 조건, 센서 상태, 관측 거리, 시야각, 과거 성능 등을 고려하여 각 센서의 신뢰도를 지속적으로 계산한다. 야간에는 카메라의 신뢰도가 감소하고, 폭우에서는 LiDAR의 신뢰도가 낮아질 수 있으며, 레이더는 물체의 재질과 형상에 따라 신뢰도가 달라질 수 있다. 이러한 동적 신뢰도 추정은 현재 환경에서 가장 신뢰할 수 있는 센서의 정보를 우선적으로 활용하도록 지원한다.



인공지능(AI)은 다중 센서 장애물 감지 기술을 크게 발전시키고 있다. 최신 딥러닝(Deep Learning) 모델은 포인트 클라우드, 영상, 레이더 신호, 깊이 정보를 동시에 입력받아 하나의 통합된 장애물 표현을 생성한다. 트랜스포머(Transformer), 그래프 신경망(Graph Neural Network), 멀티모달 파운데이션 모델(Multimodal Foundation Model)은 기존의 규칙 기반 융합을 점차 대체하며 다양한 환경에 적응할 수 있는 학습 기반 특징 표현을 제공한다.



동적 장애물 감지(Dynamic Obstacle Detection)는 다중 센서 융합의 효과가 가장 크게 나타나는 분야 중 하나이다. 이동하는 객체는 위치와 속도가 지속적으로 변하기 때문에 LiDAR의 기하학 정보, 레이더의 속도 정보, 카메라의 의미 정보, IMU의 자세 보정을 함께 이용하여 안정적인 객체 추적과 미래 이동 경로를 예측한다. 이러한 결과는 지역 경로 계획(Local Path Planning), 충돌 회피(Collision Avoidance), 행동 계획(Behavior Planning)의 신뢰성을 크게 향상시킨다.



기능 안전(Functional Safety) 관점에서 다중 센서 구조는 매우 중요한 장점을 제공한다. 일부 센서의 성능이 저하되거나 일시적으로 고장이 발생하더라도 다른 센서가 이를 보완할 수 있기 때문이다. 시스템은 센서 상태, 통신 품질, 보정 상태, 측정값의 유효성을 지속적으로 감시하며, 일부 센서에 문제가 발생해도 전체 인지 기능이 완전히 중단되지 않도록 점진적인 성능 저하(Graceful Degradation)를 구현한다.



실시간 구현(Real-Time Implementation)은 상당한 계산 자원을 요구한다. 여러 개의 고대역폭 센서 데이터를 동시에 처리해야 하기 때문이다. 효율적인 인지 시스템은 CPU, GPU, AI 가속기(AI Accelerator), 전용 하드웨어를 적절히 분산 활용한다. 병렬 처리(Parallel Processing), 비동기 실행(Asynchronous Execution), TensorRT 최적화, 관심 영역(Region of Interest) 처리, 지능형 스케줄링은 처리 지연을 최소화하면서도 높은 장애물 감지 정확도를 유지하도록 지원한다.



다중 센서 장애물 감지의 성능 평가는 단순한 검출 정확도만으로는 충분하지 않다. 센서 융합 정확도(Fusion Precision), 객체 연관 정확도, 위치 추정 오차(Localization Error), 속도 추정 품질, 의미 분류 성능, 처리 지연 시간(Latency), 센서 고장에 대한 강인성(Robustness), 계산 효율성(Computational Efficiency), 시간 동기화 정확도, 장기간 운용 안정성(Long-Term Stability) 등을 종합적으로 평가해야 한다. 다양한 환경 조건에서 수행되는 벤치마크(Benchmark)는 실제 운용 성능을 객관적으로 평가하는 중요한 기준이 된다.



현장 검증(Field Validation)은 실제 산업 및 실외 환경에서 수행되어야 한다. 정적 장애물, 이동하는 작업자, 지게차, 다른 자율주행 로봇, 차량, 반사체, 투명 물체, 악천후, 조명 변화, 센서 가림(Occlusion) 등 다양한 조건을 포함하여 시험해야 한다. 모든 센서 데이터를 시간 동기화 상태로 기록하면 오프라인 분석, 센서 보정 개선, 알고리즘 최적화, 실제 운용 중 수집된 어려운 사례를 이용한 재학습(Retraining)이 가능해진다.



미래의 다중 센서 장애물 감지 시스템은 파운데이션 모델(Foundation Model), 멀티모달 월드 모델(Multimodal World Model), 자기지도학습(Self-Supervised Learning), 적응형 센서 신뢰도 추정(Adaptive Sensor Confidence Estimation), 엣지 AI 가속(Edge AI Acceleration), 클라우드 기반 인지(Cloud-Assisted Perception)를 적극적으로 활용하게 될 것이다. 미래의 인지 시스템은 단순히 여러 센서 데이터를 결합하는 수준을 넘어 전체 3차원 환경을 이해하고, 객체의 행동을 추론하며, 미래의 상호작용을 예측하고, 실제 운용 경험을 통해 센서 간 협력을 지속적으로 최적화하게 될 것이다. 이러한 기술은 산업, 물류, 농업, 건설, 상업 및 다양한 실외 환경에서 더욱 안전하고 효율적인 자율주행 로봇을 실현하는 핵심 기반 기술이 될 것이다.



## 19.7 Safety Zone Integration

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

안전 구역 통합(안전 구역 통합, Safety Zone Integration)은 장애물 인지(Perception) 기능과 기능 안전(Functional Safety) 메커니즘을 결합하여 자율주행 모바일 로봇(Autonomous Mobile Robot, AMR)이 사람, 장비, 시설물 주변에서 안전하게 운행할 수 있도록 하는 기술이다. 장애물 감지(Obstacle Detection)가 주변 환경의 객체를 인식하는 역할을 한다면, 안전 구역 통합은 감지된 장애물이 로봇의 동작에 어떤 영향을 미쳐야 하는지를 결정한다. 이를 위해 로봇 주변에 보호 구역(Protective Region)을 정의하고, 각 구역에 적절한 안전 동작을 연결한다. 이러한 통합 과정은 인지 데이터를 산업용 안전 요구사항을 만족하는 결정론적(Deterministic) 안전 동작으로 변환하면서도 높은 작업 효율을 유지하도록 한다.



안전 구역(Safety Zone)은 로봇 주변에 미리 정의된 2차원 또는 3차원 보호 영역으로, 해당 영역 안에 장애물이 존재하면 특정한 안전 동작이 수행되도록 설계된다. 각 구역은 서로 다른 위험 수준(Risk Level)에 대응하며, 이에 따라 서로 다른 제어 동작이 적용된다. 먼 거리에서 장애물이 감지되면 단순히 감속만 수행할 수 있지만, 내부 보호 구역으로 장애물이 진입하면 제어된 정지(Control Stop) 또는 긴급 제동(Emergency Braking)이 수행된다. 적절하게 설계된 안전 구역은 불필요한 정지를 최소화하면서도 충분한 안전 여유를 확보하여 작업 효율과 안전성을 동시에 향상시킨다.



현대의 자율주행 모바일 로봇은 일반적으로 하나의 고정된 보호 구역이 아니라 여러 개의 동심원 형태(Concentric)의 안전 구역을 사용한다. 가장 바깥쪽 감시 구역(Monitoring Zone)은 장애물을 조기에 인식하여 예측 기반 계획을 가능하게 한다. 중간 경고 구역(Warning Zone)은 점진적인 감속과 경로 조정을 시작한다. 내부 보호 구역(Protective Zone)은 보호 정지(Protective Stop)를 수행하며, 가장 안쪽 긴급 구역(Emergency Zone)은 충돌 위험이 허용 가능한 수준을 초과하면 즉시 안전 정지를 수행한다. 이러한 계층형 보호 구조는 갑작스러운 긴급 정지보다 훨씬 부드러운 자율주행을 가능하게 한다.



각 안전 구역의 크기는 로봇의 동역학(Dynamics)과 운용 조건에 따라 결정된다. 최대 주행 속도(Maximum Speed), 제동 성능(Braking Capability), 적재물 질량(Payload Mass), 바닥 마찰 계수(Floor Friction), 조향 특성(Steering Characteristics), 제어기 응답 시간(Controller Response Time), 인지 지연 시간(Perception Latency), 환경 불확실성(Environmental Uncertainty) 등이 모두 안전 구역 크기에 영향을 미친다. 일반적으로 주행 속도가 증가할수록 장애물을 감지하고 데이터를 처리하며 제어 명령을 수행하고 실제로 차량을 정지시키기 위한 거리가 더 많이 필요하므로 보호 구역 역시 더욱 넓게 설정된다.



동적 안전 구역(Dynamic Safety Zone)은 현재 로봇의 운용 상태에 따라 보호 영역의 크기를 실시간으로 변경한다. 저속 정밀 도킹(Docking) 상황에서는 조향성을 높이기 위해 보호 구역을 상대적으로 작게 설정할 수 있다. 반면 고속 이송이나 실외 자율주행에서는 제동 거리가 증가하므로 동일한 로봇도 자동으로 감시 구역과 정지 구역을 확대한다. 이러한 적응형 안전 구역 관리는 현재의 위험 수준에 맞는 보호를 제공하여 안전성과 작업 효율을 동시에 향상시킨다.



장애물 감지(Obstacle Detection)는 안전 구역을 평가하기 위한 핵심 환경 정보를 제공한다. 다중 센서(Multi-Sensor) 인지 시스템은 장애물의 위치(Position), 속도(Velocity), 이동 방향(Direction), 크기(Size), 객체 종류(Classification), 신뢰도(Confidence)를 계산한다. 이후 안전 구역 관리 로직은 이러한 장애물이 로봇 주변의 어느 보호 구역과 겹치는지를 판단한다. 판단 결과에 따라 정상 주행, 속도 제한, 제어된 감속, 보호 정지, 긴급 정지(Emergency Shutdown) 등의 제어 명령이 위험 수준에 맞추어 수행된다.



사람 감지(Human Detection)는 사람의 행동이 본질적으로 예측하기 어렵기 때문에 가장 보수적인 안전 정책이 적용된다. 작업자는 갑자기 방향을 바꾸거나, 예상하지 못한 위치에서 멈추거나, 로봇의 이동 경로 안으로 갑자기 진입할 수 있다. 따라서 사람이 감지되면 정적인 구조물이나 예측 가능한 다른 자율주행 차량보다 더 넓은 안전 거리(Safety Margin), 더 낮은 허용 속도, 더 빠른 감속 전략이 일반적으로 적용된다.



장애물 분류(Obstacle Classification)는 서로 다른 객체에 서로 다른 보호 전략을 적용할 수 있도록 지원한다. 고정된 벽, 임시 팔레트(Pallet), 지게차(Forklift), 다른 자율주행 로봇, 매달린 장애물(Hanging Obstacle), 작업자(Human Worker)는 각각 다른 위험 특성을 가진다. 이러한 분류 결과를 이용하면 즉시 정지가 필요한 상황과 주의 깊은 우회(Avoidance) 또는 제한적인 상호작용이 가능한 상황을 구분할 수 있으며, 안전 규정을 만족하면서도 높은 작업 효율을 유지할 수 있다.



안전 인증 센서(Safety-Certified Sensor)는 산업용 안전 시스템에서 핵심적인 역할을 수행한다. 안전 LiDAR(Safety LiDAR)는 인증된 보호 구역을 지속적으로 감시하며 장애물이 위험 영역 안으로 들어오면 AI 시스템과 관계없이 독립적으로 보호 정지 기능을 수행한다. 이러한 인증 시스템은 국제 기능 안전 표준에 따라 검증된 결정론적 안전 성능을 제공한다. AI 기반 인지 시스템은 이러한 인증 센서를 대체하는 것이 아니라 더욱 풍부한 환경 이해와 높은 작업 효율을 제공하는 보완적인 역할을 수행한다.



다중 계층 안전 구조(Multi-Layer Safety Architecture)는 인증된 안전 하드웨어와 고급 인지 알고리즘을 함께 활용한다. 인증된 안전 센서는 AI 성능과 관계없이 최소한의 안전 기능을 보장하며, AI 인지 시스템은 의미 기반 객체 인식(Semantic Recognition), 이동 경로 예측(Trajectory Prediction), 환경 지도(Environment Mapping), 지능형 내비게이션 의사결정을 제공한다. 이러한 계층형 구조는 서로 다른 원리의 센서가 상호 감시를 수행하므로 동일 원인(Common-Mode Failure)에 의한 고장을 줄이고 단일 시스템으로는 얻을 수 없는 높은 안전성을 제공한다.



센서 융합(센서 융합, Sensor Fusion)은 개별 센서의 불확실성을 줄여 안전 구역의 신뢰성을 크게 향상시킨다. LiDAR는 정확한 거리 정보를 제공하고, 카메라는 의미 기반 객체 분류를 수행하며, 레이더(Radar)는 악천후에서도 이동 물체를 안정적으로 감지한다. 초음파 센서(Ultrasonic Sensor)는 근거리 사각지대를 보호하고, IMU(Inertial Measurement Unit)는 차량의 움직임을 보정한다. 이러한 다양한 센서를 함께 활용하면 장애물의 위치를 더욱 정확하게 계산할 수 있으며 불필요한 오경보(False Alarm)도 감소시킬 수 있다.



시간 동기화(Time Synchronization)와 센서 보정(Calibration)은 안전 구역 통합의 기본 조건이다. 모든 센서 데이터는 동일한 시점의 환경을 나타내야 하며 동일한 좌표계(Coordinate System)에서 정확하게 정렬되어야 한다. 시간 오차나 보정 오차가 발생하면 장애물이 실제 위치와 다르게 계산되어 불필요한 보호 정지가 발생하거나 더 심각하게는 위험한 장애물을 늦게 인식하는 문제가 발생할 수 있다.



가상 안전 구역(Virtual Safety Zone)은 소프트웨어 기반으로 정의되는 보호 영역이다. 출입 금지 구역, 보행자 통로, 위험 설비 주변, 충전 스테이션(Charging Station), 적재 플랫폼, 유지보수 구역 등을 디지털 지도(Digital Map)에 미리 등록할 수 있다. 로봇이 이러한 구역에 접근하면 시스템은 자동으로 속도 제한, 허용 가능한 동작, 센서 우선순위, 작업 규칙 등을 변경하여 위험이 높은 환경에서도 안전하게 운행하도록 지원한다.



상황 인식 기반 안전 구역 관리(Context-Aware Safety Zone Management)는 주변 환경의 특성을 안전 정책에 반영한다. 좁은 통로, 복잡한 교차로, 창고 통행 구역, 생산 라인, 실외 도로, 사람과 함께 작업하는 공간은 각각 서로 다른 보호 전략이 필요하다. 모든 환경에 동일한 안전 구역을 적용하는 대신 현재 위치의 특성과 작업 상황을 고려하여 안전 여유를 동적으로 조정함으로써 안전성과 작업 효율을 동시에 향상시킨다.



예측 기반 안전(Predictive Safety)은 단순한 반응형 보호를 넘어서는 중요한 발전 방향이다. 장애물이 보호 구역 안으로 들어온 이후에만 반응하는 것이 아니라 로봇과 주변 이동 객체의 미래 이동 경로를 예측한다. 충돌 예상 시간(Time-to-Collision), 이동 경로 예측(Trajectory Prediction), 행동 예측(Behavior Prediction)을 이용하여 위험한 상황이 실제로 발생하기 전에 속도를 줄이거나 이동 경로를 변경할 수 있으므로 전체 교통 흐름과 안전성이 함께 향상된다.



로봇 운동 제어(Motion Control)는 안전 구역 평가 결과와 긴밀하게 연동된다. 장애물의 위치와 예상 충돌 위험에 따라 현재 속도를 유지하거나, 점진적으로 감속하거나, 최대 속도를 제한하거나, 조향을 변경하거나, 보호 정지를 수행하거나, 긴급 제동을 실행한다. 이러한 제어 동작이 부드럽게 전환되면 탑재물 안정성(Payload Stability), 기계적 신뢰성(Mechanical Reliability), 작업 효율(Mission Efficiency)을 유지하면서도 안전성을 확보할 수 있다.



기능 안전 표준(Functional Safety Standard)은 산업용 자율주행 로봇의 안전 구역 구현에 직접적인 영향을 준다. 국제 안전 표준은 위험성 평가(Risk Assessment), 보호 정지 동작, 비상 정지(Emergency Stop), 성능 수준(Performance Level), 진단 범위(Diagnostic Coverage), 이중화(Redundancy), 고장 허용(Fault Tolerance) 등에 대한 요구사항을 정의하고 있다. 따라서 안전 구역 통합은 인지 알고리즘과 인증된 안전 제어 시스템을 결합하여 로봇의 전체 생애주기 동안 안전성과 작업 성능을 동시에 만족시켜야 한다.



시스템 상태 감시(System Health Monitoring)는 센서, 통신 네트워크, 컴퓨팅 하드웨어, 시간 동기화, 안전 제어기의 상태를 지속적으로 점검한다. 성능 저하나 하드웨어 고장이 감지되면 시스템은 자동으로 보호 구역을 확대하고, 허용 속도를 감소시키며, 일부 기능을 제한하거나, 미리 정의된 안전 상태(Safe State)로 전환한다. 이러한 점진적 성능 저하(Graceful Degradation)는 전체 인지 기능이 일시적으로 저하되더라도 안전성을 유지할 수 있도록 지원한다.



안전 구역 통합의 성능 평가는 단순한 장애물 감지 정확도만으로는 충분하지 않다. 보호 정지 거리(Protective Stopping Distance), 응답 지연(Response Latency), 불필요한 보호 정지 발생 빈도(False Protective Stop Frequency), 위험 상황 미검출 확률(Missed Hazard Probability), 안전 구역 전환의 일관성, 제어기 응답 시간, 센서 동기화 정확도, 시스템 가용성(System Availability) 등을 종합적으로 평가해야 한다. 이러한 시험을 통해 실제 운용 환경에서도 안전 동작이 예측 가능하고 반복 가능하며 신뢰성 있게 수행되는지를 검증한다.



현장 검증(Field Validation)은 작업자, 지게차, 자율주행 차량, 이동 장애물, 좁은 통로, 도킹 구역, 적재 구역, 조명 변화, 악천후, 임시 작업 환경 등을 포함하는 실제 산업 환경에서 수행되어야 한다. 엔지니어는 적응형 안전 구역이 다양한 상황에서 적절하게 동작하는지를 확인하고, 모든 센서 데이터를 시간 동기화 상태로 기록하여 오프라인 분석, 알고리즘 개선, 지속적인 성능 향상에 활용한다.



미래의 안전 구역 통합(Safety Zone Integration)은 파운데이션 모델(Foundation Model), 멀티모달 인지(Multimodal Perception), 의미 기반 월드 모델(Semantic World Model), 행동 예측(Predictive Behavioral Reasoning), 적응형 위험 평가(Adaptive Risk Assessment), 클라우드 기반 플릿 학습(Cloud-Assisted Fleet Learning)을 적극적으로 활용하게 될 것이다. 미래의 자율주행 로봇은 단순히 미리 정의된 기하학적 보호 구역에 의존하는 것이 아니라 환경의 위험도를 지속적으로 평가하고, 사람의 의도를 이해하며, 미래의 상호작용을 예측하고, 실제 운용 경험을 통해 안전 전략을 지속적으로 최적화하게 될 것이다. 이러한 기술은 산업, 물류, 의료, 건설, 농업 및 다양한 실외 환경에서 더욱 안전하고 효율적이며 지능적인 자율주행을 가능하게 하는 핵심 기반 기술이 될 것이다.



## 19.8 Obstacle Detection Testing

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

장애물 감지 시험(장애물 감지 시험, Obstacle Detection Testing)은 자율주행 모바일 로봇(Autonomous Mobile Robot, AMR)이 모든 예상 운용 환경에서 장애물을 안정적으로 감지하고, 분류하며, 추적하고, 적절하게 대응할 수 있는지를 체계적으로 검증하는 과정이다. 장애물 감지는 주행 안전, 충돌 회피(Collision Avoidance), 임무 수행(Mission Execution), 기능 안전(Functional Safety)에 직접적인 영향을 미치므로 단순히 객체 검출 정확도만 평가해서는 충분하지 않다. 종합적인 시험은 센서 하드웨어, 인지 소프트웨어, 센서 융합(Sensor Fusion), 의사결정, 시스템 지연 시간(System Latency), 환경 강인성(Environmental Robustness), 장기 운용 신뢰성(Long-Term Reliability)까지 모두 포함해야 한다. 최종 목표는 로봇이 운용 수명 전체에 걸쳐 위험 요소를 충분히 빠르게 인식하여 안전하고 효율적인 자율주행을 지속적으로 수행할 수 있음을 입증하는 것이다.



시험은 실제 실험을 수행하기 전에 명확한 시스템 요구사항(System Requirements)과 합격 기준(Acceptance Criteria)을 정의하는 것부터 시작해야 한다. 최소 검출 가능 장애물 크기(Minimum Detectable Obstacle Size), 최대 감지 거리(Maximum Detection Distance), 허용 가능한 위치 오차(Localization Error), 객체 분류 정확도(Classification Accuracy), 응답 지연 시간(Response Latency), 객체 추적 연속성(Tracking Continuity), 안전 정지 거리(Safety Stopping Distance), 운용 가능한 환경 조건(Environmental Operating Conditions), 요구되는 신뢰도 수준(Confidence Level) 등을 정량적으로 정의해야 한다. 이러한 기준은 실험실 시험과 현장 시험 모두에서 객관적인 합격 여부를 판단하는 기준이 되며, 요구사항·시험 절차·검증 결과 간의 추적성(Traceability)은 엔지니어링 품질 관리와 규제 대응을 위해 매우 중요하다.



체계적인 검증 전략(Verification Strategy)은 일반적으로 구성 요소(Component) 시험에서 시작하여 전체 시스템(System) 검증으로 점진적으로 확장된다. 먼저 개별 센서를 독립적으로 평가한 후 인지 알고리즘, 센서 융합 모듈, 내비게이션 연동, 최종 자율주행 시스템을 차례대로 시험한다. 이러한 단계적 접근은 문제 발생 시 원인을 보다 쉽게 분리하여 분석할 수 있도록 하며, 복잡한 시스템 통합 이전에 하위 하드웨어와 소프트웨어가 요구 성능을 만족하는지를 확인할 수 있다. 초기 단계에서 문제를 발견하면 전체 개발 기간과 디버깅 비용을 크게 줄일 수 있다.



센서 하드웨어 시험(Sensor Hardware Testing)은 인지 알고리즘을 평가하기 전에 각 센서가 설계 사양대로 정상 동작하는지를 확인하는 과정이다. 카메라는 영상 품질(Image Quality), 노출 안정성(Exposure Stability), 동적 범위(Dynamic Range), 색상 일관성(Color Consistency), 프레임 속도(Frame Rate), 시간 동기화 정확도를 평가한다. LiDAR는 거리 측정 정확도(Ranging Accuracy), 각도 분해능(Angular Resolution), 포인트 클라우드 밀도(Point Cloud Density), 스캔 주파수(Scan Frequency), 시야각(Field of View), 반복 측정 정확도(Repeatability)를 검증한다. 레이더(Radar)는 거리 정확도, 속도 추정 정확도(Velocity Estimation), 각도 정확도, 도플러(Doppler) 안정성, 악천후 환경에서의 감지 성능을 평가한다. 초음파 센서(Ultrasonic Sensor)는 근거리 거리 측정 정확도, 빔 특성(Beam Characteristics), 센서 간 간섭(Cross-Talk) 내성을 검증한다.



센서 보정 시험(Sensor Calibration Testing)은 내부 보정(Intrinsic Calibration)과 외부 보정(Extrinsic Calibration)이 장기간 정확하게 유지되는지를 확인하는 과정이다. 내부 보정은 렌즈 왜곡(Lens Distortion), 초점 거리(Focal Length), 센서 내부 특성을 평가하며, 외부 보정은 여러 센서와 로봇 좌표계(Robot Coordinate System) 사이의 기하학적 정렬 상태를 검증한다. 또한 진동(Vibration), 온도 변화(Thermal Cycling), 장시간 운용(Long-Duration Operation), 유지보수(Maintenance), 운송(Transportation) 이후에도 보정 상태가 유지되는지를 평가해야 한다. 작은 정렬 오차도 다중 센서 인지 정확도를 크게 저하시킬 수 있기 때문이다.



시간 동기화 시험(Time Synchronization Testing)은 모든 센서가 동일한 시간 기준으로 환경을 관측하는지를 검증하는 과정이다. 카메라, LiDAR, 레이더, IMU(Inertial Measurement Unit), GNSS(Global Navigation Satellite System), 휠 엔코더(Wheel Encoder), 안전 센서(Safety Sensor)가 허용 오차 범위 내에서 동기화되어야 한다. 정지 상태뿐 아니라 고속 주행 상황에서도 시간 동기화 정확도를 평가해야 하며, 특히 동적 장애물 감지(Dynamic Obstacle Detection), 속도 추정, 이동 경로 예측(Trajectory Prediction), 센서 융합에서는 매우 높은 시간 정확도가 요구된다.



장애물 감지 알고리즘은 우선 실험실 환경(Laboratory Environment)에서 검증되어야 한다. 실험실에서는 실험 조건을 반복 가능하게 유지할 수 있으므로 동일한 조건에서 알고리즘의 성능을 정확하게 비교할 수 있다. 크기, 형상, 재질, 위치가 알려진 인공 장애물을 이용하면 검출 정확도를 정량적으로 평가할 수 있다. 이러한 환경은 환경적 변수의 영향을 최소화하므로 소프트웨어 버전 간 성능 비교와 성능 저하(Regression) 분석에 매우 유용하다.



객체 검출 성능(Object Detection Performance)은 다양한 종류의 장애물을 얼마나 정확하게 인식하는지를 평가한다. 시험 대상에는 사람(Human), 지게차(Forklift), 자율주행 로봇, 차량(Vehicle), 팔레트(Pallet), 박스(Box), 케이블(Cable), 건설 자재, 매달린 장애물(Hanging Obstacle), 작은 잔해(Small Debris), 투명 물체(Transparent Object), 반사 표면(Reflective Surface), 불규칙한 산업 장비 등이 포함되어야 한다. 주요 평가 지표로는 정밀도(Precision), 재현율(Recall), 평균 정밀도(mAP, mean Average Precision), 오검출률(False Positive Rate), 미검출률(False Negative Rate), 위치 정확도(Localization Accuracy), 신뢰도 보정(Confidence Calibration) 등이 사용된다.



거리 기반 시험(Distance-Dependent Testing)은 센서의 전체 감지 범위에서 성능을 평가하는 과정이다. 다양한 거리에 대표 장애물을 배치하여 최소 감지 거리부터 최대 감지 거리까지 검출 확률, 위치 정확도, 객체 분류 성능, 추적 안정성을 측정한다. 이러한 성능 곡선은 최대 주행 속도에서 요구되는 안전 정지 거리를 만족할 수 있을 만큼 충분한 거리에서 장애물을 안정적으로 감지할 수 있는지를 확인하는 중요한 근거가 된다.



각도 범위 시험(Angular Coverage Testing)은 로봇 전체 시야(Field of View)에서의 감지 성능을 평가한다. 다양한 방위각(Azimuth)과 고도각(Elevation)에 장애물을 배치하여 모든 센서 영역에서 검출 성능을 비교한다. 특히 센서 사각지대(Blind Spot), 센서 간 중첩 영역, 차체 구조물에 의한 가림(Occlusion) 현상 등을 중점적으로 평가해야 한다. 이러한 시험은 회전 주행이나 복잡한 환경에서도 장애물을 지속적으로 인식할 수 있는지를 확인한다.



동적 장애물 시험(Dynamic Obstacle Testing)은 로봇과 주변 객체가 동시에 움직이는 상황에서 인지 성능을 검증한다. 보행자, 자전거, 지게차, 자율주행 로봇, 수동 운반 카트, 승용차, 건설 장비 등이 다양한 속도와 방향으로 이동하는 상황을 구성한다. 객체 추적 연속성, 속도 추정 정확도, 이동 경로 예측, 객체 ID 유지, 충돌 예측 성능 등을 실제 운용 환경과 유사한 조건에서 평가한다.



다중 센서 융합 시험(Multi-Sensor Fusion Testing)은 서로 다른 센서의 정보를 하나의 통합된 장애물 표현으로 얼마나 안정적으로 결합하는지를 검증한다. 시험 과정에서는 의도적으로 특정 센서의 성능을 저하시킨다. 예를 들어 카메라는 저조도 환경, LiDAR는 폭우, 광학 센서는 반사 표면, 통신 시스템은 전자기 간섭(Electromagnetic Interference)에 노출시킨다. 융합 알고리즘은 이러한 조건에서도 신뢰도(Confidence)에 따라 센서별 가중치를 적절히 조정하여 안정적인 인지 성능을 유지해야 한다.



소형 장애물 시험(Small Obstacle Testing)은 작은 물체가 센서에서 매우 제한된 특징만 생성하기 때문에 특별히 중요하다. 시험 대상에는 볼트(Bolt), 케이블, 호스(Hose), 떨어진 공구, 작은 박스, 돌, 나뭇가지, 연석(Curbstone), 얕은 포트홀(Pothole), 흩어진 건설 자재 등이 포함된다. 최소 검출 가능 크기, 거리별 검출 성능, 오경보(False Alarm) 발생률을 평가하며, 로봇의 지상고(Ground Clearance)와 통과 가능성(Traversability)을 고려한 내비게이션 판단이 적절하게 수행되는지도 함께 검증한다.



매달린 장애물 및 돌출 장애물 시험(Hanging and Overhanging Obstacle Testing)은 지면 위에 존재하는 3차원 장애물 인지 성능을 평가한다. 매달린 케이블, 천장 컨베이어, 낮은 교량, 배관망, 조명 장치, 경고 표지판, 나뭇가지, 적재 플랫폼 등의 구조물을 이용한다. 수직 여유 공간(Clearance) 계산 정확도, 장애물 분류 성능, 충돌 영역(Collision Envelope) 보호, 상부 구조물 아래를 통과하는 내비게이션 동작 등을 검증한다. 특히 높은 적재물이나 센서 마스트를 장착한 로봇에서는 상부 장애물 감지가 매우 중요하다.



환경 강인성 시험(Environmental Robustness Testing)은 센서 성능을 저하시킬 수 있는 다양한 운용 환경에서 인지 시스템을 평가한다. 실내에서는 조명 변화, 반사 바닥, 연기, 먼지, 복잡한 창고 환경, 좁은 통로 등을 포함하며, 실외에서는 직사광선, 그림자, 비, 안개, 눈, 바람, 물웅덩이, 진흙, 식생, 온도 변화, 비포장 노면 등을 포함한다. 이러한 시험의 목적은 실제 운용 환경에서 발생하는 다양한 조건에서도 안정적인 장애물 감지 성능을 유지하는지를 확인하는 것이다.



기능 안전 시험(Functional Safety Testing)은 인지 결과가 정의된 안전 요구사항에 따라 적절한 보호 동작을 수행하는지를 검증한다. 경고 구역(Warning Zone), 보호 정지 구역(Protective Stop Zone), 긴급 제동(Emergency Braking), 속도 제한(Speed Reduction), 안전 스캐너(Safety Scanner)와의 연동, 비상 정지(Emergency Stop), 복구 절차(Recovery Procedure)를 모두 시험한다. 다양한 접근 속도와 이동 경로에서 장애물이 진입하는 상황을 구성하여 요구된 시간 안에 안전 동작이 수행되는지와 불필요한 보호 정지가 최소화되는지를 함께 평가한다.



내비게이션 통합 시험(Navigation Integration Testing)은 장애물 감지 정보가 전체 자율주행 행동에 어떤 영향을 미치는지를 검증한다. 장애물 회피(Avoidance), 우회 경로(Rerouting), 보행자 양보(Yielding), 느린 차량 추월, 복도 주행, 도킹(Docking), 충전 스테이션 접근, 엘리베이터 진입, 적재 구역 운행, 플릿(Fleet) 운용 등을 포함하는 실제 임무를 수행한다. 이러한 시험은 인지 정보가 계획기(Planner)와 제어기(Controller)에 정확하게 전달되어 안전하고 효율적인 자율주행을 수행하는지를 확인한다.



스트레스 시험(Stress Testing)은 인지 시스템의 성능 한계를 확인하기 위해 극한 조건을 의도적으로 생성하는 시험이다. 매우 높은 장애물 밀도, 다수의 이동 객체, 부분적인 센서 고장, 네트워크 지연(Network Delay), 프로세서 과부하(Processor Overload), 위치 추정 저하, 반복적인 가림 현상, 여러 환경 교란이 동시에 발생하는 상황, 장시간 연속 운용 등을 포함한다. 이러한 시험은 이상적인 조건이 무너졌을 때 시스템이 얼마나 안정적으로 성능을 유지하는지를 평가한다.



회귀 시험(Regression Testing)은 소프트웨어 수정 이후 기존에 검증된 성능이 저하되지 않았는지를 확인하는 과정이다. 모든 인지 소프트웨어 업데이트는 객체 검출, 객체 추적, 센서 융합, 안전 동작, 내비게이션 통합, 주요 성능 지표를 포함하는 표준 시험을 자동으로 반복 수행해야 한다. 자동화된 회귀 시험은 지속적 통합(Continuous Integration) 환경에서 빠른 피드백을 제공하며 장기간의 소프트웨어 품질을 유지하는 데 매우 중요하다.



데이터 기록(Data Logging)은 모든 시험 과정에서 핵심적인 역할을 수행한다. 원시 센서 데이터(Raw Sensor Data), 중간 인지 결과, 검출된 객체, 추적 상태, 위치 추정 결과, 내비게이션 명령, 제어기 응답, 시스템 진단 정보, 시간 정보, 환경 조건 등을 모두 시간 동기화 상태로 기록해야 한다. 이러한 데이터는 오프라인 재생(Offline Replay), 시각화(Visualization), 실패 재현(Failure Reproduction), 파라미터 조정(Parameter Tuning), 원인 분석(Root Cause Analysis), 향후 알고리즘 개선에 활용된다.



성능 평가는 정량적인 지표와 정성적인 엔지니어링 평가를 함께 수행해야 한다. 정량적인 지표에는 검출 확률(Detection Probability), 정밀도, 재현율, 위치 오차, 속도 추정 오차, 추적 정확도, 응답 지연 시간, 계산 부하(Computational Load), 메모리 사용량(Memory Consumption), 전력 소비(Power Consumption), 시간 동기화 정확도, 시스템 가용성(System Availability) 등이 포함된다. 정성적인 평가는 주행의 부드러움(Navigation Smoothness), 작업자 신뢰도(Operator Confidence), 시스템 예측 가능성(Predictability), 유지보수성(Maintainability), 진단 기능(Diagnostic Capability), 장기간 자율주행 시의 전체적인 강인성(Robustness)을 평가한다.



현장 검증(Field Validation)은 상용 배치 이전의 최종 검증 단계이다. 실제 고객 환경에서 사람, 산업용 차량, 다양한 구조물, 변화하는 날씨, 조명 변화, 임시 장애물, 유지보수 작업, 장시간 임무 수행 등을 포함하여 시험해야 한다. 여러 대의 로봇을 동시에 운용하여 플릿 협업(Fleet Cooperation)과 협력형 장애물 회피(Cooperative Obstacle Avoidance)도 함께 평가한다. 이러한 현장 검증을 통해 실험실에서 얻어진 성능이 실제 운용 환경에서도 동일하게 유지되는지를 최종적으로 확인할 수 있다.



미래의 장애물 감지 시험(Obstacle Detection Testing)은 디지털 트윈(Digital Twin), 고정밀 시뮬레이션(High-Fidelity Simulation), 하드웨어-인-더-루프(Hardware-in-the-Loop, HIL), 파운데이션 모델(Foundation Model), 합성 데이터(Synthetic Data), 자동 시험 시나리오 생성(Autonomous Test Scenario Generation), 클라우드 기반 플릿 분석(Cloud-Based Fleet Analytics), 지속적 학습(Continuous Learning)을 적극적으로 활용하게 될 것이다. 미래의 검증 시스템은 사람이 직접 설계한 시험 시나리오에만 의존하지 않고 실제 운용 데이터를 이용하여 어려운 엣지 케이스(Edge Case)를 자동으로 발견하고, 새로운 시험 환경을 생성하며, 위험도를 평가하고, 시험 범위를 지속적으로 확장하게 될 것이다. 이러한 지능형 시험 체계는 차세대 자율주행 모바일 로봇의 인지 신뢰성을 더욱 높이는 동시에 전체 개발 기간을 크게 단축하는 핵심 기술이 될 것이다.
