# Upload Control for ASP.NET MVC - How to upload a file
<!-- run online -->
**[[Run Online]](https://codecentral.devexpress.com/e4381/)**
<!-- run online end -->

This example demonstrates how to use the MVC UploadControl Extension to upload files.

## Overview

Add an [upload control](https://docs.devexpress.com/AspNetMvc/DevExpress.Web.Mvc.UploadControlExtension) and specify its [Name](https://docs.devexpress.com/AspNetMvc/DevExpress.Web.Mvc.SettingsBase.Name) and [CallbackRouteValues](https://docs.devexpress.com/AspNetMvc/DevExpress.Web.Mvc.UploadControlSettings.CallbackRouteValues) properties.

```cshtml
@Html.DevExpress().UploadControl(settings => {
    settings.Name = "uc";
    settings.CallbackRouteValues = new { Controller = "Home", Action = "UploadControlCallbackAction" };
    <!-- ... -->
}).GetHtml()
```

Create a button and handle its client-side `Click` event. In the hanlder, call the upload control's `Upload` method to post the selected file to the server's memory.

```js
function OnClick(s, e) {
    uc.Upload();
}
```

```cshtml
@using(Html.BeginForm("UploadControlCallbackAction", "Home", FormMethod.Post)) {
    Html.RenderPartial("UploadControlPartial");
    @Html.DevExpress().Button(button => {
        button.Name = "btnUploadFile";
        button.Text = "Upload File";
        button.ClientSideEvents.Click = "OnClick";
    }).GetHtml()
}
```

To obtain uploaded files, call the upload control's server-side [GetUploadedFiles](https://docs.devexpress.com/AspNetMvc/DevExpress.Web.Mvc.UploadControlExtension.GetUploadedFiles.overloads) method and pass validation settings and the `UploadComplete` event handler as parameters.

```cs
public ActionResult UploadControlCallbackAction() {
    UploadControlExtension.GetUploadedFiles("uc", UploadControlDemosHelper.ValidationSettings, UploadControlDemosHelper.uc_FileUploadComplete);
    return null;
}
```

To pass information to the client, use the `e.CallbackData` argument property in the `UploadComplete` event handler. Then, handle the client-side `FileUploadComplete` event and use its `e.callbackData` argument property to get the passed information.

## Files to Review

* [HomeController.cs](./CS/Controllers/HomeController.cs) (VB: [HomeController.vb](./VB/Controllers/HomeController.vb))
* [Index.cshtml](./CS/Views/Home/Index.cshtml)
* [UploadControlPartial.cshtml](./CS/Views/Home/UploadControlPartial.cshtml)

## More Examples

* [File Upload - AJAX Uploading](https://demos.devexpress.com/MVCxFileManagerAndUploadDemos/UploadControl/DragAndDrop)
