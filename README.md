# UI_Testing
 This Package contains the required assets to have a functioning PopUp system. 
## INSTALLATION NOTES
The unity project this package is imported into needs two different Canvas to work
The first one is the UI Canvas, which will  be called **Screen Space Canvas** from now on. Make sure the render mode for it is set to Screen Space. **You will also need to tag it as "CSS".**

![alt text](https://github.com/PoliMatteo/UI_Testing/blob/main/screenshots/Canvas%20SS.png)

The second canvas is placed inside the world, and will be then called **World Space Canvas**. The render mode for this one needs to be set to World Space. **You will also need to tag it as "CWS".**

![alt text](https://github.com/PoliMatteo/UI_Testing/blob/main/screenshots/Canvas%20WS.png)

## DOCUMENTATION

### ALERT ICONS
This icon prefab can be placed anywhere within the World Space Canvas. During Runtime, selecting it will open a message PopUp. Position and rotation of the PopUp are based on the alert icon itself. It communicates with the **PopUpManager** script, found in the PopUpMessage prefab.

This prefab contains two separate scripts: *AlwayFaceCamera* and *CreatePopUp*

#### AlwayFaceCamera
A simple script that rotates the prefab to face the direction the camera is facing.

#### CreatePopUp
Implements the IPopUpCreator interface.

|   Properties   |   Type   |         |
|   ----------   |   ----   |   ---   |
|   **popUpPrefab**   |   GameObject   |   the prefab that will be instantiated   |
|   **maxActivationDistance**   | float   |   maximum distance (in units - meters -) allowed for the icon to be pressed   |

|   Functions   |   Type   |   Properties   |         |
|   ---------   |   ----   |   ----------   |   ---   |
|   **InstantiatePopUp**   |   void   |   None   |   Instantiates *popUpPrefab* if it can, then pairs it up with itself   |
|   **SetLock**   |   void   |   bool value   |   Based on the value, allows the instantiation of the prefab   |

### POPUP MESSAGE
This prefab appears when an alert icon is pressed. It can appear in both the World Space Canvas and the Screen Space Canvas, and can change Canvas in Runtime. It can be dismissed from either with the Close Button, and will then be Destroyed.



