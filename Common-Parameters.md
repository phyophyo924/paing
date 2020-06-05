## Summary

* [Layout parameters](#layout-parameters)
* [Style parameters](#style-parameters)
* [Common parameters](#common-parameters)
* [Default style](#default-style)
* [Default layout](#default-layout)

## Layout parameters

**Note**: these parameters have to be called **after** the specific parameters for each SDK **according to the following order**

| Parameter | Description |
| :------- | :--------- |
| `setCallback(static StepCallback stepCallback, static StatusCallback statusCallback, static AudioCallback audioCallback)` | Sets callbacks for the SDK |
| `StepCallback stepCallback` | Notifies all the beginnings of each step (e.g. "Frente do RG", "Verso do RG") |
| `StatusCallback statusCallback` | Notifies all changes of the status label (e.g. "Mantenha o celular parado", "Nenhum documento encontrado", among others) |
| `AudioCallback audioCallback` | Notifies when an audio starts to play, even if you set .hasSound(false) |
| `setMask(@DrawableRes int greenMask, @DrawableRes int whiteMask, @DrawableRes int redMask)` | Sets the background masks. **Please, be attented to keep the docking area of the document or face, it's very important to our face detection algorithm or document detection algorithm** |
| `setLayout(@LayoutRes int layoutId)` | Changes the [default layout](#default-layout) with another **with the same structure and cameraImageView id** |

## Style parameters

| Parameter | Description |
| :------- | :--------- |
| `hasSound(boolean enable)` | Enables/disables the sound and sound icon |
| `setStyle(@StyleRes int styleResourceId)` | Changes the [default color scheme](#default-style) with another **with the same structure** |

## Common parameters

| Parameter | Description |
| :------- | :--------- |
| `setRequestTimeout(int seconds)` | Sets the server request timeout. The default is 30 seconds for all SDKs, except 240 seconds for Onboarding |

## Default style

``` xml
<style name="baseFullStyle" parent="windowProperties">
    <item name="colorPrimary">#4CD964</item>
    <item name="colorBackgroundIcons">#E4F9E7</item>
    <item name="colorText">#FFFFFF</item>
    <item name="colorBGTop">#FF40DA94</item>
    <item name="colorBGMid">#FF3AC28F</item>
    <item name="colorBGBot">#FF2C8B83</item>
</style>

<style name="windowProperties" parent="Theme.MaterialComponents.Light">
    <item name="windowActionBar">false</item>
    <item name="windowNoTitle">true</item>
    <item name="android:windowTranslucentStatus">true</item>
    <item name="android:windowActivityTransitions">true</item>
    <item name="android:colorControlNormal">#606060</item>
    <item name="android:colorControlActivated">#606060</item>
</style>
```

## Default layout

``` xml

<?xml version="1.0" encoding="utf-8"?>
<layout>

    <data>
        <import type="android.view.View"/>

        <variable
            name="viewModel"
            type="com.combateafraude.helpers.sdk.activity.SDKViewModel" />

        <variable
            name="controller"
            type="com.combateafraude.helpers.sdk.controller.SDKController" />

    </data>

    <androidx.constraintlayout.widget.ConstraintLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_height="match_parent"
        android:layout_width="match_parent"
        android:keepScreenOn="true">

        <androidx.constraintlayout.widget.Guideline
            android:id="@+id/guidelineStart"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            app:layout_constraintGuide_percent="0.1" />

        <androidx.constraintlayout.widget.Guideline
            android:id="@+id/guidelineEnd"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            app:layout_constraintGuide_percent="0.9" />

        <androidx.constraintlayout.widget.Guideline
            android:id="@+id/guidelineTop"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:orientation="horizontal"
            app:layout_constraintGuide_percent="0.05" />

        <androidx.constraintlayout.widget.Guideline
            android:id="@+id/guidelineStatus"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:orientation="horizontal"
            app:layout_constraintGuide_percent="0.75" />

        <androidx.constraintlayout.widget.Guideline
            android:id="@+id/guidelineBottom"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:orientation="horizontal"
            app:layout_constraintGuide_percent="0.95" />

        <androidx.camera.view.PreviewView
            android:id="@id/cameraImageView"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

        <ImageView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:adjustViewBounds="true"
            android:contentDescription="@string/photo_mask"
            android:scaleType="fitXY"
            android:src="@{context.getDrawable(viewModel.maskLayout)}" />

        <ImageView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:padding="10dp"
            android:adjustViewBounds="true"
            android:contentDescription="@string/close"
            android:onClick="@{controller::onClickClose}"
            android:src="@drawable/ic_close"
            app:layout_constraintStart_toStartOf="@id/guidelineStart"
            app:layout_constraintTop_toTopOf="@id/guidelineTop" />

        <ImageView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:padding="10dp"
            android:visibility="@{viewModel.soundEnabled ? View.VISIBLE : View.GONE}"
            android:adjustViewBounds="true"
            android:contentDescription="@string/sound"
            android:src="@{viewModel.muted ? @drawable/ic_volume_off : @drawable/ic_volume_on}"
            android:onClick="@{controller::onClickSound}"
            app:layout_constraintEnd_toEndOf="@id/guidelineEnd"
            app:layout_constraintTop_toTopOf="@id/guidelineTop" />

        <androidx.constraintlayout.widget.ConstraintLayout
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:visibility="@{viewModel.statusVisibility ? View.VISIBLE : View.GONE}"
            app:layout_constraintStart_toStartOf="@id/guidelineStart"
            app:layout_constraintEnd_toEndOf="@id/guidelineEnd"
            app:layout_constraintTop_toTopOf="@id/guidelineStatus">

            <TextView
                android:id="@+id/statusMessage"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:background="@drawable/bg_radius"
                android:gravity="center_horizontal"
                android:lineSpacingExtra="7sp"
                android:padding="10dp"
                android:layout_marginTop="10dp"
                android:textAlignment="center"
                android:textColor="#606060"
                android:textSize="15sp"
                android:textStyle="bold"
                android:text="@{viewModel.statusMessage}"
                app:layout_constraintStart_toStartOf="parent"
                app:layout_constraintEnd_toEndOf="parent"
                app:layout_constraintTop_toTopOf="parent" />

            <ImageView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:background="@drawable/triangle"
                android:rotation="270"
                android:adjustViewBounds="true"
                android:contentDescription="@string/nothing"
                app:layout_constraintEnd_toEndOf="@id/statusMessage"
                app:layout_constraintStart_toStartOf="@id/statusMessage"
                app:layout_constraintTop_toTopOf="@id/statusMessage"
                app:layout_constraintBottom_toTopOf="@id/statusMessage" />

        </androidx.constraintlayout.widget.ConstraintLayout>

        <TextView
            android:id="@+id/tvCurrentStepName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            app:layout_constraintBottom_toTopOf="@id/tvPreviousStepName"
            app:layout_constraintStart_toStartOf="@id/guidelineStart"
            app:layout_constraintEnd_toEndOf="@id/guidelineEnd"
            android:layout_marginBottom="8dp"
            android:textSize="18sp"
            android:textColor="#ffffff"
            android:letterSpacing="0.06"
            android:text="@{viewModel.currentStepName}"
            android:gravity="center_horizontal" />

        <ImageView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:adjustViewBounds="true"
            android:visibility="@{viewModel.currentStepDone ? View.VISIBLE : View.GONE}"
            android:contentDescription="@string/check"
            app:layout_constraintTop_toTopOf="@id/tvCurrentStepName"
            app:layout_constraintBottom_toBottomOf="@id/tvCurrentStepName"
            app:layout_constraintEnd_toStartOf="@id/tvCurrentStepName"
            android:layout_marginEnd="8dp"
            android:src="@drawable/ic_check" />

        <TextView
            android:id="@+id/tvPreviousStepName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            app:layout_constraintStart_toStartOf="@id/guidelineStart"
            app:layout_constraintEnd_toEndOf="@id/guidelineEnd"
            app:layout_constraintBottom_toBottomOf="@id/guidelineBottom"
            android:textSize="16sp"
            android:textColor="#66FFFFFF"
            android:letterSpacing="0.06"
            android:text="@{viewModel.previousStepName}"
            android:gravity="center_horizontal" />

        <ImageView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:adjustViewBounds="true"
            android:visibility="@{viewModel.previousStepDone ? View.VISIBLE : View.GONE}"
            android:contentDescription="@string/check"
            app:layout_constraintTop_toTopOf="@id/tvPreviousStepName"
            app:layout_constraintBottom_toBottomOf="@id/tvPreviousStepName"
            app:layout_constraintEnd_toStartOf="@id/tvPreviousStepName"
            android:layout_marginEnd="8dp"
            android:alpha="0.4"
            android:src="@drawable/ic_check" />

        <ProgressBar
            android:layout_width="64dp"
            android:layout_height="64dp"
            android:indeterminate="true"
            android:indeterminateTint="?attr/colorPrimary"
            android:indeterminateTintMode="src_atop"
            android:visibility="@{viewModel.serverRequesting ? View.VISIBLE : View.GONE}"
            app:layout_constraintVertical_bias="0.45"
            app:layout_constraintBottom_toBottomOf="@id/guidelineBottom"
            app:layout_constraintTop_toTopOf="@id/guidelineTop"
            app:layout_constraintStart_toStartOf="@id/guidelineStart"
            app:layout_constraintEnd_toEndOf="@id/guidelineEnd" />

    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>

```