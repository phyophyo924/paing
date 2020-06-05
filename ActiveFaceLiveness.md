## Summary

* [Module details](#module-details)
* [Required permissions](#required-permissions)
* [Usage](#usage)

## Module details

Provides an interface used to make sure that the person in front of the device is a live person and not a photo or recording using facial movements.

## Required permissions

``` java
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.INTERNET" />
```

## Usage

To start the `ActiveFaceLivenessActivity`, first you need to create a `ActiveFaceLiveness`:

``` java
ActiveFaceLiveness mActiveFaceLiveness = new ActiveFaceLiveness.Builder("mobile_token")
    // see the table below
    .build();
```

| Parameter | Required? | Description |
| :------- | :--------- | :--------- |
| `setNumberOfSteps(int numberOfSteps)` | No | Sets the number of steps that your user must do to capture his/her selfie |
| `setActionTimeout(int seconds)` | No | Sets the time to restart the actions flow for each movement |
| [layout parameters](./Common#layout-parameters) | No | Common to all SDKs that have an activity with camera preview |
| [style parameters](./Common#style-parameters) | No | Common to all SDKs that have an activity |
| [common parameters](./Common#common-parameters) | No | Common to all SDKs |

After that, you can start the `ActiveFaceLivenessActivity` by passing this object by parameter in the caller intent:

``` java
Intent mIntent = new Intent(context, ActiveFaceLivenessActivity.class);
mIntent.putExtra(ActiveFaceLiveness.PARAMETER_NAME, mActiveFaceLiveness);
startActivityForResult(mIntent, REQUEST_CODE);
```

To get the `ActiveFaceLivenessResult`, override `onActivityResult` method in the caller activity:

``` java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    if (requestCode == REQUEST_CODE){
        if (resultCode == RESULT_OK && data != null) {
            ActiveFaceLivenessResult mActiveFaceLivenessResult = (ActiveFaceLivenessResult) data.getSerializableExtra(ActiveFaceLivenessResult .PARAMETER_NAME);
            // check the mActiveFaceLivenessResult.getSDKFailure() here to know for which reason the SDK had finished
        } else {
            // the user closes the activity
        }
    }
    super.onActivityResult(requestCode, resultCode, data);
}
```

The `ActiveFaceLivenessResult` class has the following fields:

| Field | Description |
| :------- | :--------- |
| `@Nullable String imagePath` | The full path of selfie that are storaged inside the device. Will be null in a failure reason |
| `int missedAttemps` | Value that counts each wrong action made by user |
| `@Nullable SDKFailure sdkFailure` | [SDKFailure](./SDKFailure) |