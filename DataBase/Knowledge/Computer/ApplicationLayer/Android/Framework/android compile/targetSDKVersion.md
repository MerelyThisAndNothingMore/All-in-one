---
tags: 
alias:
---
##### minSdkVersion

> 指定能够运行应用的最低 API 级别。默认值为“1”。
> 
> 应用在 android:minSdkVersion 中声明 API 级别的主要原因是，告知 Android 系统，其正使用在指定 API 级别引入的 API。如果由于某种原因将应用安装在 API 级别较低的平台上，则它会在运行时试图访问不存在的 API 时发生崩溃。如果应用所需的最低 API 级别高于目标设备上平台版本的 API 级别，则系统不允许安装该应用，以防出现这种结果。

##### targetSdkVersion

> 指定运行应用的目标 API 级别。在某些情况下，此属性允许应用使用在目标 API 级别中定义的清单元素或行为，而非仅限于使用针对最低 API 级别定义的元素或行为。
> 
> targetSdkVersion 属性不会阻止您的应用安装在高于指定值的平台版本上，但它很重要，因为它向系统指示您的应用是否应继承较新版本中的行为更改。如果您不将 targetSdkVersion 更新到最新版本，则系统会认为您的应用在最新版本上运行时需要一些向后兼容性行为。例如，在 Android 4.4 中的行为更改中，使用 AlarmManager API 创建的闹钟现在默认不精确，因此系统可以批量处理应用闹钟并节省系统电量，但如果您的目标 API 级别低于“19”，则系统会为您的应用保留之前的 API 行为。





targetSdkVersion指定的值表示你在该目标版本上已经做过了充分的测试,系统将会为你的应用程序启用一些最新的功能和特征。