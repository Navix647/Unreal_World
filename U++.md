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
含义：指定自定义Getter函数
使用场景：复杂属性逻辑
具体案例：
1.开关灯
-   **如果不使用 Setter**：蓝图用户设置 `bIsLightOn = true`，然后必须手动调用一个 `UpdateLightState()` 函数，灯才会亮。如果用户忘了调函数，就是 Bug。
-   **使用 Setter**：蓝图用户只需设置 `bIsLightOn = true`，灯自动亮起。
```c++
// .h 
UPROPERTY(EditAnywhere, BlueprintReadWrite, BlueprintSetter=SetIsLightOn) 
bool bIsLightOn; 
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
2.获取角色的速度 (Speed)
-   我们通常存储的是 `Velocity` (向量)，但策划和美术通常只关心 `Speed` (标量)。
    
-   不需要专门存一个 `Speed` 变量，只需要写一个 Getter。

```c++
// .h 
// 注意：这里没有 EditAnywhere，因为它是算出来的，不能编辑 
UPROPERTY(BlueprintReadOnly, BlueprintGetter=GetSpeed) 
float Speed; 
UFUNCTION(BlueprintGetter) 
float GetSpeed() const; 
// .cpp 
float AMyCharacter::GetSpeed() const 
{ 	// 实时计算 return GetVelocity().Size(); }

```




注意事项：需要对应的UFUNCTION
### BlueprintGetter

### EditAnywhere

### VisibleAnywhere

## UFUNCTION
### BlueprintCallable
含义：函数可在蓝图中调用，有执行引脚



### BlueprintPure
含义：纯函数，无执行引脚，只读取数据


### BlueprintThreadSafe


## UMETA

## GENERATE_BODY()



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk5MzAwMjYxNiwtNjE3NzU4MDU3LDIyOT
Q4OTY0OCwxNjc1MDQwNTI3LC02OTgwMzc2NTEsMTQzMDI3NDkw
OSwxMzQ3MTE3NjQ1LDEzMDE3NTkzNzMsMTYwOTgwODAyMSw3Mz
A5OTgxMTZdfQ==
-->