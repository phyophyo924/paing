## Summary

* [Module details](#module-details)
* [Required permissions](#required-permissions)
* [Usage](#usage)

## Module details

Plug-and-play integration with `DocumentDetector`, one of the `FaceLiveness` SDK and our [API](https://docs.combateafraude.com/docs/conhecendo-produto/visao-geral/)

## Required permissions

``` java
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.INTERNET" />
```

## Usage

To start the `OnboardingActivity`, first you need to create a `Onboarding`:

``` java
Onboarding mOnboarding = new Onboarding.Builder("api_token", "mobile_token")
    // see the table below
    .build();
```

| Parameter | Required? | Description |
| :------- | :--------- | :--------- |
| `setCpf(String cpf)` | Yes | Sets the user CPF used in fallback case on document OCR process |
| `setReportId(String cafReportId)` | Yes | Sets which API report the execution will be created |
| `setFaceLivenessMode(FaceLivenessMode mode)` | Yes | Sets which liveness mode will be used. Both means your user will choose it |
| [style parameters](./Common#style-parameters) | No | Common to all SDKs that have an activity |
| [common parameters](./Common#common-parameters) | No | Common to all SDKs |

After that, you can start the `OnboardingActivity` by passing this object by parameter in the caller intent:

``` java
Intent mIntent = new Intent(context, OnboardingActivity.class);
mIntent.putExtra(Onboarding.PARAMETER_NAME, mOnboarding);
startActivityForResult(mIntent, REQUEST_CODE);
```
To get the `OnboardingResult`, override `onActivityResult` method in the caller activity:

``` java
@Override
protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
    if (requestCode == REQUEST_CODE){
        if (resultCode == RESULT_OK && data != null){
            OnboardingResult mOnboardingResult = (OnboardingResult) data.getSerializableExtra(OnboardingResult.PARAMETER_NAME);
            // check the mOnboardingResult.getSDKFailure() here to know for which reason the SDK had finished.
        } else {
            // the user closed the activity
        }
    }
    super.onActivityResult(requestCode, resultCode, data);
}
```

The `OnboardingResult` class has the following fields:

| Field | Description |
| :------- | :--------- |
| `@Nullable String apiToken` | The API token passed by parameter |
| `@Nullable String reportId` | The report ID passed by parameter |
| `@Nullable String executionId` | The execution ID created in API |
| `@Nullable String documentFrontPath` | The full path of document front storaged inside the device |
| `@Nullable String documentBackPath` | The full path of document back storaged inside the device |
| `@Nullable String selfiePath` | The full path of selfie storaged inside the device |
| `@Nullable SDKFailure sdkFailure` | [SDKFailure](./SDKFailure) |