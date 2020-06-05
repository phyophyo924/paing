This page explains each reasons that can finish the SDK, encapsulated in `SDKFailure` object.

| Instance of | Meaning | Example Case | SDKs |
| :--: | :--: | :--: | :--: |
| `null` | The SDK flow was successful | When user finish the SDK with success | DocumentDetector<br>ActiveFaceLiveness<br>PassiveFaceLiveness<br>FaceAuthenticator<br>Onboarding |
| `InvalidTokenReason` | mobile token is invalid for the correspondent SDK | Passing "test123" in SDK Builder | DocumentDetector<br>ActiveFaceLiveness<br>PassiveFaceLiveness<br>FaceAuthenticator<br>Onboarding |
| `PermissionReason` | Missing runtime permissions | Start DocumentDetector without camera permission | DocumentDetector<br>ActiveFaceLiveness<br>PassiveFaceLiveness<br>FaceAuthenticator<br>Onboarding |
| `NetworkReason` | Internet connection failure during any SDK request | Connection lost in FaceAuthenticator checking | DocumentDetector<br>ActiveFaceLiveness<br>PassiveFaceLiveness<br>FaceAuthenticator<br>Onboarding |
| `ServerReason ` | When a SDK request receives an unsuccessful HTTP status code | In theory, this should never happen | DocumentDetector<br>ActiveFaceLiveness<br>PassiveFaceLiveness<br>FaceAuthenticator<br>Onboarding |
| `StorageReason` | The user doesn't have internal storage space | No storage space when try to take a document photo | DocumentDetector<br>ActiveFaceLiveness<br>PassiveFaceLiveness<br>FaceAuthenticator<br>Onboarding |
| `LibraryReason` | When any SDK internal library can't start | Forgetting the `noCompress` configuration will let this failure in DocumentDetector | DocumentDetector<br>DeviceAuthenticator |
| `GPSFailureReason` | When the SDK can't collect the device location | Device has fake GPS or turned off the GPS | AddressCheck |