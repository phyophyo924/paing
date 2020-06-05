## Summary

* [Module details](#module-details)
* [Required permissions](#required-permissions)
* [Usage](#usage)

## Module details

Provide an interface used to capture predefined documents with quality and assurance that facilitates the process of Optical Character Recognition (OCR).

## Required permissions

``` xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.INTERNET" />
```

## Usage

To start the `DocumentDetectorActivity`, first you need to create a `DocumentDetector`:

``` java
DocumentDetector mDocumentDetector = new DocumentDetector.Builder("mobile_token")
    // see the table below
    .build();
```

| Parameter | Required? | Description |
| :------- | :--------- | :--------- |
| `setDocumentDetectorFlow(DocumentDetectorStep[] flow)` | Yes | Sets the array of documents to capture. Can be DocumentDetector.CNH_FLOW, DocumentDetector.RG_FLOW or any custom flow |
| `showPopup(boolean show)` | No | Shows/hides the document popup that helps the client |
| [layout parameters](./Common#layout-parameters) | No | Common to all SDKs that have an activity with camera preview |
| [style parameters](./Common#style-parameters) | No | Common to all SDKs that have an activity |
| [common parameters](./Common#common-parameters) | No | Common to all SDKs |

The `DocumentDetectorStep` constructor has the following structure:

``` java
DocumentDetectorStep mDocumentDetectorStep = new DocumentDetectorStep(
    Document scanClassification,
    @StringRes int stepLabel,
    @DrawableRes int illustration,
    @RawRes int audio,
    @StringRes int notFoundMessage
);
```

| Parameter | Required? | Description | Default |
| :------- | :--------- | :--------- | :--------- |
| `Document scanClassification` | Yes | Sets which document will be scanned in the correspondent step | |
| `@StringRes int stepLabel` | No | Sets the String label that will be shown in the bottom of screen | "Documento" |
| `@DrawableRes int illustration` | No | Sets the illustration that will be shown in the popup | An CPF illustration |
| `@RawRes int audio` | No | Sets the audio that will be played in the start of step | An audio that says "Documento" |
| `@StringRes int notFoundMessage` | No | Sets the String label that will be shown when the user aim the camera to anything except the correct document | "Documento não encontrado" |

After that, you can start the `DocumentDetectorActivity` by passing this object by parameter in the caller intent:

``` java
Intent mIntent = new Intent(context, DocumentDetectorActivity.class);
mIntent.putExtra(DocumentDetector.PARAMETER_NAME, mDocumentDetector);
startActivityForResult(mIntent, REQUEST_CODE);
```

To get the `DocumentDetectorResult`, override `onActivityResult` method in the caller activity:

``` java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    if (requestCode == REQUEST_CODE){
        if (resultCode == RESULT_OK && data != null){
            DocumentDetectorResult documentDetectorResult = (DocumentDetectorResult) data.getSerializableExtra(DocumentDetectorResult.PARAMETER_NAME);
            // check the documentDetectorResult.getSDKFailure() here to know for which reason the SDK had finished.
        } else {
            // the user closes the activity
        }
    }
    super.onActivityResult(requestCode, resultCode, data);
}
```

The `DocumentDetectorResult` class has the following fields:

| Field | Description |
| :------- | :--------- |
| `@Nullable Capture[] captures` | The array of document captures. It will be the same length of flow passed by parameter |
| `@Nullable SDKFailure sdkFailure` | [SDKFailure](./SDKFailure) |

The `Capture` class has the following fields:

| Field | Description |
| :------- | :--------- |
| `@NonNull String imagePath` | The full path of document storaged inside the device |
| `int missedAttemps` | Value that counts each wrong document aimed by user |