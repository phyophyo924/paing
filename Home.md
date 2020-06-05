Welcome to the Combate a Fraude's Android SDKs wiki! Start the integration [here](./Downloading-and-importing)!

|          SDK            |                       Gradle dependency                       |
| ----------------------- | ------------------------------------------------------------- |
| Onboarding              | `com.combateafraude.sdk:onboarding:1.0.0-beta9`               |
| DocumentDetector        | `com.combateafraude.sdk:document-detector:1.0.0-beta12`       |
| PassiveFaceLiveness     | `com.combateafraude.sdk:passive-face-liveness:1.0.0-beta13`   |
| ActiveFaceLiveness      | `com.combateafraude.sdk:active-face-liveness:1.0.0-beta11`    |
| FaceAuthenticator       | `com.combateafraude.sdk:face-authenticator:1.0.0-beta10`      |
| DeviceAuthenticator     |  soon       |
| AddressCheck            |  soon       |

## Release notes

### Update - June 04, 2020

- **Added**: support to other documents in DocumentDetector, like Registro Nacional de Estrangeiros (RNE), Carteira de Identidade de Advogado (OAB) and Identidade Militar.
- **Changed**: constructor of DocumentDetector. Now you can start it with your own flow!
- **Removed**: the .setConfidence() parameter in DocumentDetector.Builder. There is no reason to allow the user to edit it.

### Update - May 29, 2020

- **Removed**: the parameter .setMaxDimensionPhoto(). If you want smaller image sizes, compress it in the app size. Be careful, low quality documents can result in future OCR problems!
- **Added**: parameter .showPopup() in DocumentDetector

### Update - May 27, 2020

- **Improvement**: now the DocumentDetector scanning area is larger, improving the quality of the document photo
- **Improvement**: better distinction in the DocumentDetector step changing (the change from front to back of the document)
- **Changed**: algorithm of face detection. Now, the UX is so much better! (if you find a bad UX or bug, please mail to [Head of Mobile](mailto:frederico.gassen@combateafraude.com)!)
- **Changed**: from 1,5s to 2,5s of device stabilization in the DocumentDetector, in favor of camera autofocus for better quality photos
- **Changed**: centralized the callback and mask parameters into one method for each

### Update - May 20, 2020

- **Changed**: Onboarding SDK requires the CPF in the Builder
- **Changed**: Onboarding Builder now has the FaceLivenessMode.BOTH, in case you want to let your user choose it
- **Changed**: you now can pass null parameters to the SDK Builder. This will be useful in case to call all builder modifiers with setted or not setted values

### Update - May 07, 2020 - :warning: BREAKING CHANGES :warning:

- **Changed** the URL to our maven repository changed from `maven { url "https://raw.githubusercontent.com/combateafraude/Mobile/releases" }` to `maven { url "https://raw.githubusercontent.com/combateafraude/Mobile/android-releases" }`. We'll remove the old URL soon, so, please use the new.
- **Improvement** the device sensors were optimized, so, you will note that the SDKs are faster.

### Update - May 04, 2020

- **Changed** layout files to use [DataBinding](https://developer.android.com/topic/libraries/data-binding) to better maintain the SDKs. **Now the apps must enable dataBinding and use [this](https://gist.github.com/kikogassen/62068b6e5bc7988d28594d833b125519) layout template.**
- **Removed** the popups in the SDKs (Document popups and the initial popup)

### Update - April 29, 2020

- **Added** missedAttemps value in return of DocumentDetector, PassiveFaceLiveness and ActiveFaceLiveness

### Update - April 28, 2020

#### Onboarding version 1.0.0-beta2
- **Changed** buttons to Button class

#### DocumentDetector version 1.0.0-beta2
- **Added** new parameters .showDialog() and .setMaxDimensionPhoto()
- **Removed** the popup in SDK initialization
- **Changed** now the red mask is only applied to ERROR occurrences (e.g. wrong document)
- **Changed** now the sound is played only on steps initialization

#### PassiveFaceLiveness version 1.0.0-beta2
- **Added** new parameter .setMaxDimensionPhoto()
- **Removed** the popup in SDK initialization
- **Changed** now the red mask is only applied to ERROR occurrences (e.g. invalid face)
- **Changed** now the sound is played only on steps initialization

#### ActiveFaceLiveness version 1.0.0-beta2
- **Added** new parameter .setMaxDimensionPhoto()
- **Removed** the popup in SDK initialization
- **Changed** now the red mask is only applied to ERROR occurrences (e.g. wrong action)
- **Changed** now the sound is played only on steps initialization

#### FaceAuthenticator version 1.0.0-beta2
- **Added** new parameter .setMaxDimensionPhoto()
- **Removed** the popup in SDK initialization
- **Changed** now the red mask is only applied to ERROR occurrences
- **Changed** now the sound is played only on steps initialization

### Update - April 23, 2020

#### ActiveFaceLiveness version 1.0.0-beta1
- **Added** ensures that the movements are in correct order

### Update - April 09, 2020
- **Removed** `CAF_` from SDK names

### Update - April 03, 2020

#### CAF_Onboarding version 1.0.6-beta
- **Added** anti spoofing algorithm. Now the user can't register with a fake photo, like 3d mask
- **Added** parameter .setLogoId() to set the logo shown in the RequestActivity
- **Added** parameter .setFaceLivenessMode() to choose what mode you want to do faceliveness
- **Added** field selfieUrl in OnboardingResult class if you want to catch the photo URL
- **Changed** SDKError to SDKFailure in OnboardingResult class.
- **Changed** default requestTimeout from 60 to 240
- **Removed** userId field in OnboardingResult class. Now, to do the authentication, you no longer need this parameter.

#### CAF_DocumentDetector version 1.0.6-beta
- **Changed** SDKError to SDKFailure in DocumentDetectorResult class.

#### CAF_FaceLivenessEffortless version 1.0.0-beta
- **NEW** SDK to ensure the face liveness without facial movements

#### CAF_FaceLivenessMotion version 1.0.6-beta
- **Changed** changed CAF_FaceLiveness to CAF_FaceLivenessMotion due the new SDK called CAF_FaceLivenessEffortless
- **Changed** SDKError to SDKFailure in FaceLivenessMotionResult class
- **Changed** no longer need server request in this SDK
- **Removed** removed the userId field in the FaceLivenessMotionResult

#### CAF_FaceAuthenticator version 1.0.6-beta
- **Added** anti spoofing algorithm. Now the user can't authenticate with a fake photo, like 3d mask or picture of a picture
- **Added** this SDK now requires READ_PHONE_STATE permission
- **Changed** the parameter .setUserId() to .setCpf(). The CPF will be the authentication key
- **Changed** SDKError to SDKFailure in FaceAuthenticatorResult class

#### CAF_Security version 1.0.6-beta
- **Changed** DeviceInformation class to Device class
- **Added** getBasicDeviceInformations method in the Security class
- **Added** getFullDeviceInformations method in the Security class

### Update - March 25, 2020

#### CAF_Onboarding version 1.0.5-beta
- **Changed** reduce the upload time in 60%
- **Changed** StatusCode to SDKError in the OnboardingResult. For more information, please check [this page](https://github.com/combateafraude/Android/wiki/Common#sdkerror).
- **Changed** default requestTimeout from 30 to 60

#### CAF_DocumentDetector version 1.0.5-beta
- **Changed** StatusCode to SDKError in the DocumentDetectorResult. For more information, please check [this page](https://github.com/combateafraude/Android/wiki/Common#sdkerror).
- **Changed** default requestTimeout from 15 to 30

#### CAF_FaceLiveness version 1.0.5-beta
- **Changed** StatusCode to SDKError in the FaceLivenessResult. For more information, please check [this page](https://github.com/combateafraude/Android/wiki/Common#sdkerror).
- **Changed** default requestTimeout from 15 to 30

#### CAF_FaceAuthenticator version 1.0.5-beta
- **Changed** StatusCode to SDKError in the FaceAuthenticatorResult. For more information, please check [this page](https://github.com/combateafraude/Android/wiki/Common#sdkerror).
- **Changed** default requestTimeout from 15 to 30

#### CAF_Security version 1.0.5-beta
- No changes

### Update - March 23, 2020

#### CAF_Onboarding version 1.0.4-beta
- No changes

#### CAF_DocumentDetector version 1.0.4-beta
- **Changed** increased quality of images

#### CAF_FaceLiveness version 1.0.4-beta
- **Changed** increased quality of images
- **Changed** reduced the time to detect face movements

#### CAF_FaceAuthenticator version 1.0.4-beta
- **Changed** increased quality of images

### Update - March 16, 2020

#### CAF_Onboarding version 1.0.3-beta
- **Changed** Now the Onboarding.Builder.setCpf() is optional, like the web system.

#### CAF_DocumentDetector version 1.0.3-beta
- **Added** Possibility to parameterize the layout of activity
- **Added** Possibility to disable the activity sounds
- **Added** Possibility to send parameter callbacks for each step started, status changed and sound played
- **Changed** Activity keeps the screen on

#### CAF_FaceLiveness version 1.0.3-beta
- **Added** Possibility to parameterize the layout of activity
- **Added** Possibility to disable the activity sounds
- **Added** Possibility to send parameter callbacks for each step started, status changed and sound played
- **Changed** Activity keeps the screen on

#### CAF_FaceAuthenticator version 1.0.3-beta
- **Added** Possibility to parameterize the layout of activity
- **Added** Possibility to disable the activity sounds
- **Added** Possibility to send parameter callbacks for each step started, status changed and sound played
- **Changed** Activity keeps the screen on

#### CAF_Security version 1.0.3-beta
- No changes

### Update - March 10, 2020

#### CAF_Onboarding version 1.0.2-beta
- No changes

#### CAF_DocumentDetector version 1.0.2-beta
- **Changed** CNH back is easier to scan
- **Changed** "Encaixe o documento na máscara" by "Afaste um pouco o celular"

#### CAF_FaceLiveness version 1.0.2-beta
- No changes

#### CAF_FaceAuthenticator version 1.0.2-beta
- No changes

#### CAF_Security version 1.0.2-beta
- No changes

### Update - March 5, 2020

#### CAF_Onboarding version 1.0.1-beta
- **Changed** Removed the activity actionBar 

#### CAF_DocumentDetector version 1.0.1-beta
- **Changed** Removed the activity actionBar 

#### CAF_FaceLiveness version 1.0.1-beta
- **Changed** Removed the activity actionBar 

#### CAF_FaceAuthenticator version 1.0.1-beta
- **Changed** Removed the activity actionBar 

#### CAF_Security version 1.0.1-beta
- No changes

## iOS <img src="https://github.com/combateafraude/Mobile/blob/master/resources/apple.png">

