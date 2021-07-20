#### 位置约束

```xml
layout_constraintLeft_toLeftOf
layout_constraintLeft_toRightOf
layout_constraintRight_toLeftOf
layout_constraintRight_toRightOf
layout_constraintTop_toTopOf
layout_constraintTop_toBottomOf
layout_constraintBottom_toTopOf
layout_constraintBottom_toBottomOf
layout_constraintBaseline_toBaselineOf
layout_constraintStart_toEndOf
layout_constraintStart_toStartOf
layout_constraintEnd_toStartOf
layout_constraintEnd_toEndOf
```

#### Margins

值必须 >= 0

```
android:layout_marginStart
android:layout_marginEnd
android:layout_marginLeft
android:layout_marginTop
android:layout_marginRight
android:layout_marginBottom
 [cs.html](../../../Downloads/cs.html) 
```

```
layout_goneMarginStart
layout_goneMarginEnd
layout_goneMarginLeft
layout_goneMarginTop
layout_goneMarginRight
layout_goneMarginBottom
layout_goneMarginBaseline
```

#### bias

```
layout_constraintHorizontal_bias
layout_constraintVertical_bias
```

#### gone

ConstraintLayout 中 Visibility 为 GONE 的子 View 特点:

1. 布局时, 该子view会被当做一个点(宽高都是0)
2. 自身位置约束仍然生效,但是所有 margin 都会变成0

#### 尺寸约束

###### 最大最小尺寸(WRAP_CONTENT 时才会生效)

```
android:minWidth set the minimum width for the layout
android:minHeight set the minimum height for the layout
android:maxWidth set the maximum width for the layout
android:maxHeight set the maximum height for the layout
```

###### 宽高约束 三种方式

1. 固定大小 (80dp)
2. WRAP_CONTENT
3. 0dp (MATCH_CONSTRAINT)

###### WRAP_CONTENT

自己测量, 结果无限制, 可能会超出 parent

如果想让大小限制在 parent内, 可以使用

```
app:layout_constrainedWidth="true|false"}
app:layout_constrainedHeight="true|false"
```

###### MATCH_CONSTRAINT

默认会占据所有可用空间, 可以加一下几个约束条件:

```
layout_constraintWidth_min and layout_constraintHeight_min : will set the minimum size for this dimension
layout_constraintWidth_max and layout_constraintHeight_max : will set the maximum size for this dimension
layout_constraintWidth_percent and layout_constraintHeight_percent : will set the size of this dimension as a percentage of the parent
```

###### Ratio

```
layout_constraintDimensionRatio
```

The ratio can be expressed either as:

- a float value, representing a ratio between width and height
- a ratio in the form "width:height"

###### Chains





