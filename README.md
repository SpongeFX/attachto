# <img src="https://static.sidefx.com/images/apple-touch-icon.png" width="25" height="25" alt="Hbuild Logo"> Attachto

![license](https://img.shields.io/badge/license-MIT-green) ![version](https://img.shields.io/badge/version-1.0-blue) 

### Attachto is a Houdini SideFX digital asset for adding points to polygons by clicking the LMB in the viewport.

[![demo](https://github.com/SpongeFX/attachto/assets/152398516/14e018c5-3a46-4a1c-b85a-49ac369cdaed)](https://github.com/SpongeFX/attachto/assets/152398516/83950ac4-68c1-496c-8878-b8612fd5da0c)


Attachto creates points when the LMB is clicked in the viewport on polygonal geometry objects and attaches these points to the polygons on which they were created, inheriting all subsequent transformations of the parent polygons. By using with "CopyToPoints" SOP node this asset allows for quick and convenient attachment of instanced geometry to other polygonal geometry objects, creating groups, and controlling orientation and position relative to the parent objects.


## 💾  Installation 
[Download the HDA file](otls/attachto.hdalc) and install it to your `houdini19.5/otls/` folder. For detailed instructions, please refer to the [Houdini documentation](https://www.sidefx.com/docs/houdini/assets/install.html)

## ☑️ Features
+ Tested in Houdini versions: 19.5
+ Object Space.
+ Group and individual parameter settings
+ Extracting transformation matrix from input polygons
+ Control of the addition of newly created points to groups
+ Control and randomization of the <i>@pscale</i> attribute for each created point
+ Control of rotation axis and rotation angles and randomization of rotation angles when using the @orient attribute
+ Control of position changing relative to the parent object
+ Alignment of instanced geometric objects copied onto points along the surface of polygons

## 🏃 Quick guide how to use the Attachto

#### Requirements for incoming geometry:
* Does not work with packed geometry
* The polygonal object to which points need to be attached must contain a primitive attribute <i>@xform</i> with a transformation matrix4 for each primitive:
  * If you are using packed geometry, create the <i>@xform</i> attribute using the "getpackedtransform" function and unpack the packed polygons. 
  * If you are using the "Transform" SOP node, enable the "Output Attribute" toggle, add the "attribpromote" SOP node, and transfer the detail attribute <i>@xform</i> to the primitives.
  * If you cannot create the <i>@xform</i> attribute on incoming primitives, add a "connectivity" SOP node before the asset, go to the "Incoming Geometry" tab, enable the corresponding toggle, and set the attribute name the same as specified in the "connectivity" SOP node.

<br>

1. Connect the polygonal object to "input1".

2. Select the HDA "attachto" in the Network View, select "Show Handle" in the selector and handle control menu.

3. Set the necessary parameters if required. Click the mouse to add points to the polygonal geometry object connected to "input1" and displayed in the viewport.

<br>

<table border="1">
  <tbody>
    <tr>
      <td width="25%" rowspan="5"><img src="https://github.com/SpongeFX/attachto/assets/152398516/785476cf-ab83-4667-ba6f-11ad51179e6a"></td>
      <td>Use the <i><b>Reset</b></i> button to reset parameters and delete points. This works when the asset is selected in the Network View.</td>
    </tr>
    <tr>
      <td>The "<i><b>Number Of Points</b></i>" parameter specifies the number of points you plan to set. Do not input numbers greater than 1000, as this may cause temporary freezing if your computer is not powerful enough.</td>
    </tr>
    <tr>
      <td>Between the reset button and the "Number Of Points" parameter, there is a "<i><b>counter</b></i>" that shows how many points you can add to the geometry.</td>
    </tr>
    <tr>
      <td>Once the planned number of points has been added, you can increase the value of the "Number Of Points" parameter if you need to continue adding points, or press the reset button to delete all set points.</td>
    </tr>
    <tr>
      <td>The <i><b>Clear</b></i> button resets the value of the "Number Of Points" parameter.</td>
    </tr>
  </tbody>
</table>

<br>

[![Video Quick Guide](https://github.com/SpongeFX/attachto/assets/152398516/9f515e21-2af9-4bf9-90fc-61329ace46f6)](https://youtu.be/iIrEpRk16cI "A brief overview of the available functionality of the hda")

<br>

#### 💡 Tips for using 
* If you already know the polygons to which you need to attach points, try to input only those into the asset, removing all others. This will increase efficiency and reduce the use of computational resources.
* Points are added quite quickly, and if you need to place a single point in a specific position, try not to move the mouse cursor during the click. If someone finds this control inconvenient to use, please let me know, and I will add control over the delays between adding points to the asset parameters.


<br>


## 📂 Tabs and Parameters

### 📁 Default Settings

"Default Setting" tab contains basic parameters for creating points and controlling attribute values.<br>
The majority of parameters in default settings are presets for individual parameters of newly created points that are used during calculations.

<br>
<table border="1">
    <tr>
        <td valign="top" width="18%"><i>Group</i></td>
        <td><p>Adding created points to groups. Enabling the toggle activates the "Group Name" parameter and allows adding newly created points to a group with the name specified in the "Group Name" parameter. Changing group membership and group name is available at any time in the individual parameters of each created point.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Сopy Normal From Primitive To Points</i></td>
        <td><p>Copies the <i>Normal</i> attribute from the primitive at the intersection point where the point was created to the created point.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Offsets For Added Points</i></td>
        <td><p>Offsets the position of created points in the direction of the normal of the primitive on which the point was created. After toggling on, it activates the "Offset" parameter, the value of which serves as a preset for newly created points. Each newly created point will be shifted by the value specified in this parameter in the direction of the normal of the primitive on which the point was created or in another direction if the "Along Toggle Axis" toggle is enabled. Changing the offset parameter value is available at any time in the individual parameters of each created point. This does not work when the "Align To Primitive" toggle is enabled.</p></td>
    </tr>
    <tr>
        <td valign="top"><a id="delay"></a><i>Along Custom Axis</i>
    </td>
        <td>Changing the direction vector for offsetting the position of points. Available when the "Custom Axis" or "Look At" toggle is enabled. The new value will be taken from the "Axis" or "Look At" vector parameter.</td>
    </tr>
    <tr>
        <td valign="top"><i>Offset Multiplier</i></td>
        <td valign="top">Offsets the position of all created points in the direction of the normal of the primitive on which the point was created. Enabling the toggle activates the "Multiplier" parameter, which serves as a multiplier for the values of the individual "Offset #" parameter for all created points. If values other than 0 were previously specified in the individual "Offset #" parameter for the points, these values will be taken into account in the offset calculations.<br></td>
    </tr>
    <tr>
        <td valign="top"><i>Attribute (float)</i></td>
        <td><p>Adds a float point attribute to the created points. After toggling on, each newly created point will be assigned an attribute with the name specified in the "Attribute name" parameter and the value specified in the "Attribute Value" parameter. Points created before toggling have an attribute value of "0". To set attribute values for previously created points, use the "Apply To All" button. The "Apply To All" button assigns the value of the "Attribute Value" parameter to the corresponding individual parameter for all created points. By default, an attribute named <i>@pscale</i> with a value of "1" is created.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Attribute Randomize</i></td>
        <td><p id="dlim">Randomizes values of the floating-point attribute for newly created points. Use this with the <i>@pscale</i> attribute to randomize sizes of geometry copied onto points.</td>
    </tr>
    <tr>
        <td valign="top"><i>Align To Prim</i></td>
        <td><p>Aligns geometric objects copied onto points to the plane of the primitives. For proper operation, set the height of the object copied onto points in the "Instance Height" parameter. With one height value in the "Instance Height" parameter, you can adjust different object sizes by changing the individual parameter "Attribute Value" for each point, provided the attribute name is <i>@pscale</i>. When randomization is enabled, alignment is performed considering the modified values of <i>@pscale</i>. To compensate for polygon overlap, toggle on "Offset Multiplier" and add the required offset in the "Multiplier" parameter.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Rotation</i></td>
        <td><p>Enabling adds a rotation angle to the quaternion <i>@orient</i> and allows rotating the attached geometry along the axis directed along the primitive normal or as specified by the user. Rotation affects all created points but can be set for each point in its individual parameters. To do this, go to the desired point, activate the "individual Rotation Angle" parameter using the "Activate" toggle, and set the required value for the individual rotation angle.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Randomize Rotation</i></td>
        <td><p>Randomizes rotation angle values. Use this if you want to randomize the rotation angle for all points.</p></td>
    </tr>
    <tr>
        <td ><i>Custom Axis</i></td>
        <td><p>Sets a custom rotation axis for all points. Activates the vector parameter to set a custom axis rotation direction. By default, the rotation axis is equal to the primitive normal on which the point was created.</p></td>
    </tr>
    <tr>
        <td ><i>Look At</td>
        <td><p>Sets a custom rotation axis vector directed from the position where the primitive was intersected to the position specified in the vector parameter "Look At Position".</p></td>
    </tr>
</table><br>
<a id="explode"></a> 

### 📁 Incomming Geomerty

This tab contains settings necessary to obtain the transformation matrix from incoming polygons. Use this if your geometry does not contain a primitive attribute <i>@xform</i> with a transformation matrix4.

<table border="1">
    <tr>
        <td valign="top" width="18%"><a id="expmode"></a><i>Get XFORM</i></td>
        <td><p>Enabling activates the calculation of the transformation matrix4 using a faster but less efficient method. This works well with objects that do not deform (do not change their sizes) over time. For proper operation, specify the attribute name containing the object identifier in the "Attribute name" parameter.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Get XFORM for each primitive</i></td>
        <td><p>Enabling activates the calculation of the transformation matrix4 for each primitive. This method works with deforming objects. It is a more efficient method but may take more time. For proper operation, specify the attribute name containing the object identifier in the "Attribute name" parameter.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Pieces Name</i></td>
        <td><p>Attribute name containing the object identifier.</p></td>
    </tr>
</table><br>
<a id="ric"></a> 

## 💬 Feedback

If you have any feedback or run into issues, please feel free to open an issue on this GitHub project. I appreciate your support!
