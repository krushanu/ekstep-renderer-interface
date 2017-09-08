# ekstep-renderer-interface
renderer interface for HTML content

# Introduction:

ContentRenderer(GenieCanvas) has bundled with GenieService & TelemetryService. To log telemetry events, ContentRenderer is using TelemetryService. To get content specific API call, ContentPlayer is using GenieService in mobile/App and V3 API's when it is embedded in web preview(editor & portal).

HTML content can't use these service directly because ContentRenderer is launching HTML content in IFrame. Iframe will not give direct access to its parent. To access the parent(ContentPlayer) services, HTML content has to bundled with RendererInterface file(javascript file).

Note: [Deprecated HTML GenieService bridge](https://github.com/ekstep/Common-Design/wiki/HTML-GenieService-Bridge:-Invoke-GenieService-from-HTML-content#genieservice-methods)

## RendererInterface
RendererInterface is a javascript class, helps to communicate with the content renderer when the content is launched in Iframe.
RendererInterface class variables & methods.

### Variables

| **Variable** | **Description**|
|---------- | ----------- |
| **parent**| To access content renderer methods or variables |
| **EkstepRendererAPI**| To access ContentRenderer APIs |

### Functions/APIs

| **Function** | **Description**|
|---------- | ----------- |
| **dispatchEvent**| Dispatch event to communicate with ContentRenderer |
| **getcontentMetadata**| Get current content metadata details |
| **getConfig**| Get content renderer configuration |
| **logTelemetryInteract**| Log temetry interact(OE_INTERACT) event |
| **logTelemetryXapi**| log telemetry xapi(OE_XAPI) event (Used only for H5P contents) |
| **gotoEndPage**| Show content renderer end-page after completion of game |
| **isMobile**| Content renderer launch mode(launhced in mobile or web) |
| **exit**| Exit from current content |

## How to Use
Add the following to your HTML/application:

The file_path is the relative path (eg. assets/js) to these files within the HTML content.
```js
  <!-- Renderer Interface -->
  <script src="[relative_path]/RendererInterface.js"></script>
```

Onload event handler of the HTML page, you can access RendererInterface variables or functions using `RI` or `org.ekstep.contentrenderer.interface`. This is the instance of RendererInterface class.

To log Telemetry events:
User can use telemetry events exposed by renderer directly
```js
RI.logTelemetryInteract(data);
```

To log telemetry events which are not exposed by RendereInterface, Use the below workaround to log other Telemetry events
```js
var TelemetryService = RI.parent.TelemetryService;

// To log OE_ITEMRESPONSE option selection in assessment
TelemetryService.itemResponse(data);

// To log OE_ERROR when any interrupt happened
TelemetryService.error(data)
```

To use [ContentRenderer service methods](https://github.com/ekstep/Common-Design/wiki/TelemetryV2-manager-methods), to get content specific data  

```js
var contentService = RI.EkstepRendererAPI.getService();  

// To get Content metadata 
contentService.getContent(contentId)
      .then(function(result) {
        // genieservice getcontent call success(success callback function)
        console.log("Success - getContent", result);
        var currentContent =  result;
        return currentContent;
      })
      .catch(function(err){
        console.log("Failed", err);
      }) 
```

### Reference Docs: 
[Ekstep Renderer API documentation](https://community.ekstep.in/developer-documentation/renderer-plugins/docs/EkstepRendererAPI.html)

[TelemetrySpec documentation](https://community.ekstep.in/specifications-guides/53-telemetry-specification)

[ContentRenderer TelemetryService Methods](https://github.com/ekstep/Common-Design/wiki/TelemetryV2-manager-methods)


