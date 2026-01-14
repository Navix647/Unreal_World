 七个最主要的宏标签：





## UClass
### BlueprintType 几乎必选
对于希望在蓝图中交互的类，这个必选

### Blueprintable 经常选
若还希望蓝图能继承它，必须加上

### NotBlueprintType 默认选项

### NotBlueprintable 默认选项


## UEnum
### BlueprintType

## USTRUCT
### BlueprintType

## UPROPERTY
### BlueprintReadOnly
含义：属性在蓝图中只读

### BlueprintReadWrite
含义：属性在蓝图中可读可写


### BlueprintSetter
含义：指定自定义Setter函数
使用场景：属性验证、副作用
注意事项：需要对应的UFUNCTION
具体案例：
**1.副作用**
解释：这是最常用的场景。当你在蓝图中修改一个变量时，你希望不仅仅是改变数值，还能**立刻**看到游戏世界的变化（比如改变材质、播放音效、更新变换）。

场景举例：开关灯 
-   **如果不使用 Setter**：蓝图用户设置 `bIsLightOn = true`，然后必须手动调用一个 `UpdateLightState()` 函数，灯才会亮。如果用户忘了调函数，就是 Bug。
-   **使用 Setter**：蓝图用户只需设置 `bIsLightOn = true`，灯自动亮起。
```c++
// .h 
UPROPERTY(EditAnywhere, BlueprintReadWrite, BlueprintSetter=SetIsLightOn) 
bool bIsLightOn; 

// 对应的UFUNCTION
UFUNCTION(BlueprintSetter) 
void SetIsLightOn(bool bNewState); 

// .cpp 
void AMyActor::SetIsLightOn(bool bNewState) 
{ 
	bIsLightOn = bNewState; // 副作用：直接修改组件状态 
	LightComponent->SetVisibility(bIsLightOn); // 副作用：甚至可以播放声音 
	if(bIsLightOn) UGameplayStatics::PlaySoundAtLocation(...); 
}
```

**2.属性验证**
解释：防止蓝图设计者输入非法数值，破坏游戏逻辑。

场景举例：数据校验与钳制 
-   生命值不能超过最大值，也不能低于 0。
    
-   如果没有 Setter，蓝图开发者可能会不小心把 HP 设为 9999 或者 -10。
```c++
// .h
UPROPERTY(EditAnywhere, BlueprintReadWrite, BlueprintSetter=SetIsLightOn)
bool bIsLightOn;
// 对应的UFUNCTION
UFUNCTION(BlueprintSetter)
void SetIsLightOn(bool bNewState);

// .cpp
void AMyActor::SetIsLightOn(bool bNewState)
{
    bIsLightOn = bNewState;
    // 副作用：直接修改组件状态
    LightComponent->SetVisibility(bIsLightOn); 
    // 副作用：甚至可以播放声音
    if(bIsLightOn) UGameplayStatics::PlaySoundAtLocation(...);
}
```


### BlueprintGetter
含义：指定自定义Getter函数
使用场景：复杂属性逻辑
具体案例：
**1.伪装成变量的“计算属性” (Computed Properties)**
解释：有些数据并不需要存储在内存里，它是实时计算出来的，但为了让蓝图节点看起来简洁，我们把它伪装成一个变量。
场景举例：获取角色的速度 (Speed)
-   我们通常存储的是 `Velocity` (向量)，但策划和美术通常只关心 `Speed` (标量)。
    
-   不需要专门存一个 `Speed` 变量，只需要写一个 Getter。

```c++
// .h 
// 注意：这里没有 EditAnywhere，因为它是算出来的，不能编辑 
UPROPERTY(BlueprintReadOnly,VisibleAnywhere, BlueprintGetter=GetSpeed) 
float Speed; 
// 对应的UFUNCTION
UFUNCTION(BlueprintGetter) 
float GetSpeed() const; 
// .cpp 
float AMyCharacter::GetSpeed() const 
{ 	
	// 实时计算 
	return GetVelocity().Size(); 
}
```
在蓝图中，用户看到的是一个名为“Speed”的绿色变量节点，而不是一个函数调用节点，连线更干净。

**2.懒加载 (Lazy Initialization)**
用于优化性能。只有当蓝图真正去读取这个变量时，才去执行加载或查找逻辑。
**场景举例：获取 UI 组件**
-   假设有一个复杂的 UI Widget，但不是每次都需要。如果一开始就 `CreateWidget` 会卡顿。
    
-   使用 Getter，在第一次访问时才创建。

```c++
// .h
UPROPERTY(BlueprintReadOnly, BlueprintGetter=GetInventoryWidget)
UUserWidget* InventoryWidget;

UUserWidget* InternalWidgetInstance; // 私有缓存

UFUNCTION(BlueprintGetter)
UUserWidget* GetInventoryWidget();

// .cpp
UUserWidget* AMyHUD::GetInventoryWidget()
{
    if (!InternalWidgetInstance)
    {
        // 只有在第一次被蓝图读取时才创建
        InternalWidgetInstance = CreateWidget<>(...);
    }
    return InternalWidgetInstance;
}
```
### EditAnywhere

### VisibleAnywhere

## UFUNCTION
### BlueprintCallable
含义：函数可在蓝图中调用，有执行引脚



### BlueprintPure
含义：纯函数，无执行引脚，只读取数据


### BlueprintThreadSafe



## UINTERFACE
和UCLASS一样都是用来放在类上面的



## *UMETA

## GENERATE_BODY()



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTMyNTgyOTIzMiwtNTgwMTA1MDMyLDIwNT
U5MzcyNDQsLTE1NDMyNjQ0ODgsLTYxNzc1ODA1NywyMjk0ODk2
NDgsMTY3NTA0MDUyNywtNjk4MDM3NjUxLDE0MzAyNzQ5MDksMT
M0NzExNzY0NSwxMzAxNzU5MzczLDE2MDk4MDgwMjEsNzMwOTk4
MTE2XX0=
-->