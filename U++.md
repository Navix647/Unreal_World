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
### BlueprintCallable
含义：函数可在蓝图中调用，有执行引脚



### BlueprintPure
含义：纯函数，无执行引脚，只读取数据


### BlueprintThreadSafe


## UMETA

## GENERATE_BODY()



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQxNzMxMDYyNSwyMjk0ODk2NDgsMTY3NT
A0MDUyNywtNjk4MDM3NjUxLDE0MzAyNzQ5MDksMTM0NzExNzY0
NSwxMzAxNzU5MzczLDE2MDk4MDgwMjEsNzMwOTk4MTE2XX0=
-->