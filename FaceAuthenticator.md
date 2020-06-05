## Summary

* [Module details](#module-details)
* [Required permissions](#required-permissions)
* [Usage](#usage)

## Module details

Provides an interface used to capture a selfie and compare it with a server-storaged image, making sure they are the same person.

## Required permissions 

``` java
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
```

## Usage

To start the `FaceAuthenticatorActivity`, first you need to create a `FaceAuthenticator`:

``` java
FaceAuthenticator mFaceAuthenticator = new FaceAuthenticator.Builder("mobile_token")
    // see the table below
    .build();
```

| Parameter | Required? | Description |
| :------- | :--------- | :--------- |
| `setCpf(String cpf)` | Yes | Sets the CPF to server get the registered user selfie to compare |
| [layout parameters](./Common#layout-parameters) | No | Common to all SDKs that have an activity with camera preview |
| [style parameters](./Common#style-parameters) | No | Common to all SDKs that have an activity |
| [common parameters](./Common#common-parameters) | No | Common to all SDKs |

After that, you can start the `FaceAuthenticatorActivity` by passing this object by parameter in the caller intent:

``` java
Intent mIntent = new Intent(context, FaceAuthenticatorActivity.class);
mIntent.putExtra(FaceAuthenticator.PARAMETER_NAME, mFaceAuthenticator);
startActivityForResult(mIntent, REQUEST_CODE);
```

To get the `FaceAuthenticatorResult`, override `onActivityResult` method in the caller activity:

``` java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    if (requestCode == REQUEST_CODE) {
        if (resultCode == RESULT_OK && data != null) {
            FaceAuthenticatorResult mFaceAuthenticatorResult = (FaceAuthenticatorResult) data.getSerializableExtra(FaceAuthenticatorResult.REQUEST_CODE);
            // check the mFaceAuthenticatorResult.getSDKFailure() here to know for which reason the SDK had finished
        } else {
            // the user closes the activity
        }
    }
    super.onActivityResult(requestCode, resultCode, data);
}
```

The `FaceAuthenticatorResult` class has the following fields:

| Field | Description |
| :------- | :--------- |
| `boolean authenticated` | A boolean that indicates if the user selfie is the same of server-storaged with the correspondent CPF |
| `@Nullable SDKFailure sdkFailure` | [SDKFailure](./SDKFailure) |