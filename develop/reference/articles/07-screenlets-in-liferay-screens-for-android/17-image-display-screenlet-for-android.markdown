# Image Display Screenlet for Android [](id=image-display-screenlet-for-android)

## Requirements [](id=requirements)

- Android SDK 4.0 (API Level 15) or above
- Liferay 7.0 CE, Liferay DXP
- Liferay Screens Compatibility Plugin
  ([CE](http://www.liferay.com/marketplace/-/mp/application/54365664) or 
  [DE](http://www.liferay.com/marketplace/-/mp/application/54369726), 
  depending on your Liferay edition). This app is preinstalled in Liferay 7.0 CE 
  and Liferay DXP instances.

## Compatibility [](id=compatibility)

- Android SDK 4.0 (API Level 15) and above

## Features [](id=features)

Image Display Screenlet displays an image file from a Liferay instance's 
Documents and Media Library. 

## Module [](id=module)

- None

## Views [](id=views)

- Default

The Default View uses an Android `ImageView` to display the image. 

![Figure 1: Image Display Screenlet using the Default View.](../../images/screens-android-imagedisplay.png)

## Offline [](id=offline)

This Screenlet supports offline mode so it can function without a network 
connection. For more information on how offline mode works, see the 
[tutorial on its architecture](/develop/tutorials/-/knowledge_base/7-0/architecture-of-offline-mode-in-liferay-screens). 
Here are the offline mode policies that you can use with this Screenlet: 

| Policy | What happens | When to use |
|--------|--------------|-------------|
| `REMOTE_ONLY` | The Screenlet loads the data from the Liferay instance. If a connection issue occurs, the Screenlet uses the listener to notify the developer about the error. If the Screenlet successfully loads the data, it stores it in the local cache for later use. | Use this policy when you always need to show updated data, and show nothing when there's no connection. |
| `CACHE_ONLY` | The Screenlet loads the data from the local cache. If the data isn't there, the Screenlet uses the listener to notify the developer about the error. | Use this policy when you always need to show local data, without retrieving remote information under any circumstance. |
| `REMOTE_FIRST` | The Screenlet loads the data from the Liferay instance. If this succeeds, the Screenlet shows the data to the user and stores it in the local cache for later use. If a connection issue occurs, the Screenlet retrieves the data from the local cache. If the data doesn't exist there, the Screenlet uses the listener to notify the developer about the error. | Use this policy to show the most recent version of the data when connected, but show an outdated version when there's no connection. |
| `CACHE_FIRST` | The Screenlet loads the data from the local cache. If the data isn't there, the Screenlet requests it from the Liferay instance and notifies the developer about any errors that occur (including connectivity errors). | Use this policy to save bandwidth and loading time in case you have local (but probably outdated) data. |

## Required Attributes [](id=required-attributes)

- `entryId`

If you don't use `entryId`, you must use both of the following attributes: 

- `className`
- `classPK`

## Attributes [](id=attributes)

| Attribute | Data type | Explanation |
|-----------|-----------|-------------|
| `layoutId` | `@layout` | The layout to use to show the View. |
| `autoLoad` | `boolean` | Whether the image automatically loads when the Screenlet appears in the app's UI. The default value is `true`. |
| `entryId` | `number` | The primary key of the image. | 
| `className` | `string` | The image's fully qualified class name. Since files in a Documents and Media Library are `DLFileEntry` objects, their `className` is [`com.liferay.document.library.kernel.model.DLFileEntry`](@platform-ref@/7.0-latest/javadocs/portal-kernel/com/liferay/document/library/kernel/model/DLFileEntry.html). The `className` and `classPK` attributes are required to instantiate the Screenlet. |
| `classPK` | `number` | The image’s unique identifier. The `className` and `classPK` attributes are required to instantiate the Screenlet. |
| `cachePolicy` | `string` | The offline mode setting. See [the Offline section](/develop/reference/-/knowledge_base/7-0/image-display-screenlet-for-android#offline) for details. |
| `imageScaleType` | `number` | Lets you set a scale image type like `CENTER`, `CENTER_CROP`, `CENTER_INSIDE`, `FIT_CENTER`, `FIT_END`, `FIT_START`, `FIT_XY`, `MATRIX`. |
| `placeHolder` | `@resource` | Image to load until the final image loads. |
| `placeHolderScaleType` | `number` | Lets you set a scale image type for the placeholder like `CENTER`, `CENTER_CROP`, `CENTER_INSIDE`, `FIT_CENTER`, `FIT_END`, `FIT_START`, `FIT_XY`, `MATRIX`. |

Note that the values for `imageScaleType` and `placeHolderScaleType` match those 
[described in Android's `ImageView.ScaleType`](https://developer.android.com/reference/android/widget/ImageView.ScaleType.html). 

## Listener [](id=listener)

Because images are assets, Image Display Screenlet delegates its events to a 
class that implements `AssetDisplayListener`. This interface lets you implement 
the following methods: 

- `onRetrieveAssetSuccess(AssetEntry assetEntry)`: Called when the Screenlet 
  successfully loads the image. 

- `error(Exception e, String userAction)`: Called when an error occurs in the 
  process. The `userAction` argument distinguishes the specific action in which 
  the error occurred. 
