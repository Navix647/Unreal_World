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
```c++
UPROPERTY(BlueprintGetter=GetHealth, BlueprintSetter=SetHealth)
float Health;


```
注意事项：需要对应的UFUNCTION
### BlueprintGetter

### EditAnywhere

### VisibleAnywhere

## UFUNCTION

### BlueprintPure


### BlueprintThreadSafe


## UMETA

## GENERATE_BODY()



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjI5NDg5NjQ4LDE2NzUwNDA1MjcsLTY5OD
AzNzY1MSwxNDMwMjc0OTA5LDEzNDcxMTc2NDUsMTMwMTc1OTM3
MywxNjA5ODA4MDIxLDczMDk5ODExNl19
-->