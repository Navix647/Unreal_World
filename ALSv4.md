参考一下这个网站：
https://ue5study.com/unrealengine-physics/#toc8
上面有大量先进技术的介绍和实现

```mermaid
classDiagram
class PlayerCameraManager

class CameraSkeletalMesh

class CameraSkeleton

PlayerCameraManager *-- CameraSkeletalMesh

class CameraAnimBP

CameraSkeletalMesh --> CameraAnimBP

CameraSkeleton <-- CameraSkeletalMesh

class ALSPlayerController

class ALS_BaseCharacter

class ALS_AnimManCharacter

ALS_AnimManCharacter <|-- ALS_BaseCharacter
ALSPlayerController --> PlayerCameraManager
ALS_AnimManCharacter ..|> BPI_Camera~Inteface~
ALS_BaseCharacter ..|> BPI_Camera~Inteface~
ALSPlayerController --> ALS_AnimManCharacter

class EmbeddedCameraCurve{
CameraOffsetX
CameraOffsetY
CameraOffsetZ
PivotLagSpeedX
PivotLagSpeedY
PivotLagSpeedZ
}

class BPI_Camera~Inteface~{
(transform)BPI_GetThirdPersonPivot()
(TPFOV,FPFOV,isRightShoulder)BPI_GetCameraParams()
(TraceOrigin,TraceRadius,EnumTraceType)BPI_GetThirdPersonTraceTarget()
(vector)BPI_GetFirstPersonLocation()
}


CameraSkeleton *-- EmbeddedCameraCurve

CameraAnimBP --> EmbeddedCameraCurve

CameraAnimBP --> BPI_Camera~Inteface~
```



摄像机系统

ALSPlayerController是最顶层的，它直接引用PlayerCameraManager，从PlayerController事件触发，事件控制时将


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU2OTI1MTg5NCwtMTA0Njk4ODA1NF19
-->