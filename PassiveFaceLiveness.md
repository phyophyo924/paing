## Summary

* [Module details](#module-details)
* [Required permissions](#required-permissions)
* [Usage](#usage)

## Module details

Provides an interface used to make sure that the person in front of the device is a live person and not a photo or recording using artificial intelligence.

## Required permissions

``` java
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.INTERNET" />
```

## Usage

To start the `PassiveFaceLivenessActivity`, first you need to create a `PassiveFaceLiveness`:

``` java
PassiveFaceLiveness mPassiveFaceLiveness = new PassiveFaceLiveness.Builder("mobile_token")
    // see the table below
    .build();
```

| Parameter | Required? | Description |
| :------- | :--------- | :--------- |
| [layout parameters](./Common#layout-parameters) | No | Common to all SDKs that have an activity with camera preview |
| [style parameters](./Common#style-parameters) | No | Common to all SDKs that have an activity |
| [common parameters](./Common#common-parameters) | No | Common to all SDKs |

After that, you can start the `PassiveFaceLivenessActivity` by passing this object by parameter in the caller intent:

``` java
Intent mIntent = new Intent(context, PassiveFaceLivenessActivity.class);
mIntent.putExtra(PassiveFaceLiveness.PARAMETER_NAME, mPassiveFaceLiveness);
startActivityForResult(mIntent, REQUEST_CODE);
```

To get the `PassiveFaceLivenessResult`, override `onActivityResult` method in the caller activity:

``` java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    if (requestCode == REQUEST_CODE){
        if (resultCode == RESULT_OK && data != null) {
            PassiveFaceLivenessResult mPassiveFaceLivenessResult = (PassiveFaceLivenessResult) data.getSerializableExtra(PassiveFaceLivenessResult.PARAMETER_NAME);
            // check the mPassiveFaceLivenessResult.getSDKFailure() here to know for which reason the SDK had finished
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
| `@Nullable String imageUrl` | The URL of selfie that are storaged in cloud. It only exists cause the SDK already upload the image, so, you don't need to upload it in your side if you don't want |
| `int missedAttemps` | Value that counts each spoofing tried by user |
| `@Nullable SDKFailure sdkFailure` | [SDKFailure](./SDKFailure) |