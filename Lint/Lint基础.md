## 原生 issure

自带Lint在`BuiltinIssueRegistry`类中注册

<br>

ObsoleteLintCustomCheck

> 版本过时，可根据实际情况选择是否需要更新版本，如果需要这修改lint版本，不需要这忽略

<br>

Lint ID：ManifestOrder

> Manifest配置项顺序问题，如：将<uses-feature>移动到<application>前即可

<br>

Lint ID：ProtectedPermissions

> 申请了系统权限，若已知需要使用系统权限则忽略此告警

<br>

LintID：UnusedResources

> 存在未使用的资源，删除未使用的资源即可

<br>

Lint ID：MissingSuperCall 

> 重写了父类方法后，确保是否需要调用super

<br>

Lint ID：NestedScrolling

> 更改布局，防止scroll嵌套

<br>

Lint ID：ScrollViewSize

> ScrollView 的子布局 match_parent 无效，可改用 wrap_content

<br>

LintId: DuplicateIncludedIds

> id命名重复，更改id名称可解决，不同布局文件中相同id名称可忽略

LintId：Instantiatable

> AndroidManifest.xml中定义的Activity未继承Activity，有些Activity类继承了DataBinding相关类，可忽略

<br>

LintId：InvalidWakeLockTag

> tag建议使用特定格式：唯一前缀加冒号，如：AppName:SoundRecorder

<br>


LintId：MissingConstraints

> ConstraintLayout布局中的View没有做约束，需要添加一下约束条件

<br>

LintId：Range

> 值范围问题，如值需要大于等0

<br>

LintId：SdCardPath

> 路径不建议直接写成sdcard，应该通过相应Api获取外置存储路径，如: 

```kotlin
/*
* 该方法会获取到外部存储的根目录[/storage/emulated/0]，如果要使用需要获取外置存储权限
* 在Android10已经不允许使用android.permission.WRITE_EXTERNAL_STORAGE权限，
* 但可以通过在apllication标签下添加android:requestLegacyExternalStorage="true"来设定允许使用
* 但在Android11已经完全不能使用该权限
**/
Environment.getExternalStorageDirectory().absolutePath
```

<br>

LintId：UnusedAttribute

> 有些属性只有在特定SDK版本下才有效

<br>

LintId: FragmentLiveDataObserve

> 生命周期不匹配，比如在Fragment的onViewCreated中订阅观察对象，但被观察对象使用的是Fragment的LifecycleOwner 即从onCreate到onDestroty，就会出现该警告。 可使用`viewLifecycleOwner`代替LifecycleOwner

```java
GlobalCache.getInstance().getCCMainFragmentShowLD().observe(fragment.getViewLifecycleOwner(), mainShowObserver);
```

## 自定义 issure

Lint ID：GsonLint

Lint Class：GsonDetector

> 使用 JsonModel.parseType 代替 Gson自带的 fromJson 方法，如：

```kotlin
val jsonData = JsonModel.parseType(jsonString, JsonData::class.java)
```

<br>


Lint ID：ParseXXXLint

Lint Class：ParseXXXDetector

> 使用 StringUtil 类里的 safeCast 系列方法代替原生的 Parse 系列类型转换方法，如：

```kotlin
val r = StringUtil.safeCastInt("1")
```

<br>

LintID：ThrowablePrintLint

LintClass：ThrowablePrintDetector

> 在 try-catch 中，使用 CLog 代替 e.printStackTrace 输出 throwable 信息

## 自定义 Lint

可通过以下几种方式进行Lint检查：

### 1. 返回方法名称

> 主要涉及方法为 getApplicableMethodNames 和 visitMethodCall

实例如下：

```kotlin
class LogDetector : Detector(), Detector.UastScanner {
    companion object {
        private const val TARGET_CLASS : String = "android.util.Log"
        private const val ISSUE_ID = "LogLint"
        private const val ISSUE_DESCRIPTION = "不建议使用Log打印信息"
        private const val ISSUE_EXPLANATION = "请使用CLog代替Log打印信息"
        private const val ISSUE_PRIORITY = 6

        val issue: Issue = Issue.create(
            ISSUE_ID,
            ISSUE_DESCRIPTION,
            ISSUE_EXPLANATION,
            Category.CORRECTNESS,
            ISSUE_PRIORITY,
            Severity.WARNING,
            Implementation(LogDetector::class.java, JAVA_FILE_SCOPE)
        )
    }
    override fun getApplicableMethodNames(): List<String> {
        println("LogDetector | getApplicableMethodNames")
        return listOf("d", "v", "i", "w", "e")
    }

    override fun visitMethodCall(
        context: JavaContext,
        node: UCallExpression,
        method: PsiMethod
    ) {
        super.visitMethodCall(context, node, method)
        // 方法名称
        println("LogDetector | visitMethodCall, ${node.methodName}")
        // 方法返回类型
        println("LogDetector | visitMethodCall, ${node.returnType?.canonicalText}")
        // 方法名称
        println("LogDetector | visitMethodCall, ${method.name}")
        // 方法返回类型
        println("LogDetector | visitMethodCall,  ${method.returnType?.canonicalText}")
        if (!context.evaluator.isMemberInClass(method, TARGET_CLASS)) {
            return
        }
        context.report(issue, context.getLocation(node), ISSUE_DESCRIPTION)
    }
}
```

### 2. 返回构造方法类型

> 主要设计方法 getApplicableConstructorTypes 和 visitConstructor

使用实例：

```kotlin
package com.example.limtlib.detector

import com.android.tools.lint.detector.api.*
import com.intellij.psi.PsiMethod
import org.jetbrains.uast.UCallExpression

class ThreadDetector : Detector(), Detector.UastScanner {
    
    companion object {
        private const val CONSTRUCT_NAME = "java.lang.Thread"
        private const val ISSUE_ID = "ThreadLint"
        private const val ISSUE_DESCRIPTION = "建议为线程设置名称"
        private const val ISSUE_EXPLANATION = """建议为线程设置名称, 如：Thread("name")"""
        private const val ISSUE_PRIORITY = 6
        private const val ARGUMENT_TYPE = "java.lang.String"

        val issue: Issue = Issue.create(
            ISSUE_ID,
            ISSUE_DESCRIPTION,
            ISSUE_EXPLANATION,
            Category.CORRECTNESS,
            ISSUE_PRIORITY,
            Severity.WARNING,
            Implementation(ThreadDetector::class.java, Scope.JAVA_FILE_SCOPE)
        )
    }
    override fun getApplicableConstructorTypes(): List<String> {
        println("ThreadDetector | getApplicableConstructorTypes")
        return listOf(CONSTRUCT_NAME)
    }

    override fun visitConstructor(
        context: JavaContext, 
        node: UCallExpression, 
        constructor: PsiMethod
    ) {
        super.visitConstructor(context, node, constructor)
        val arguments = node.valueArguments
        // 构造函数名称
        println("ThreadDetector | visitConstructor, ${constructor.name}")
        var isReport = true
        arguments.forEach {
            // 参数值
            println("ThreadDetector | visitConstructor, ${it.evaluate()}")
            val type = it.getExpressionType()
            // 参数类型
            println("ThreadDetector | visitConstructor, ${type?.canonicalText}")
            if (type?.canonicalText == ARGUMENT_TYPE) {
                isReport = false
                return@forEach
            }
        }
        if (isReport) {
            context.report(LogDetector.issue, context.getLocation(node), ISSUE_DESCRIPTION)
        }
    }
}
```

创建Detector类后，需要进行注册：

```kotlin
package io.linlangli.lint_jar   

import com.android.tools.lint.client.api.IssueRegistry
import com.android.tools.lint.client.api.Vendor
import com.android.tools.lint.detector.api.Issue

import com.android.tools.lint.detector.api.CURRENT_API
import io.linlangli.lint_jar.detector.LogerDetector

@Suppress("UnstableApiUsage")
class DetectorRegister : IssueRegistry() {
    override val api: Int
        get() = CURRENT_API
    override val issues: List<Issue>
        get() = listOf(LogerDetector.ISSURE)
    override val minApi: Int
        get() = 8
    override val vendor: Vendor?
        get() = Vendor(
            vendorName = "Android Open Source Project",
            feedbackUrl = "https://com.example.lint.blah.blah",
            contact = "author@com.example.lint"
        )
}
```