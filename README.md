# POPUPS AND OTHER ICONS
 This Package contains the required assets to have a functioning PopUp system.
 TextMeshPro is required to be in the project
## INSTALLATION NOTES
Download the *WarningPopUps.unitypackage* file and open it into any Unity project.

The unity project this package is imported into needs two different Canvas to work.
The first one is the UI Canvas, which will  be called **Screen Space Canvas** from now on. Make sure the render mode for it is set to Screen Space - Overlay. **You will also need to tag it as "CSS".**

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
If it can, instantiates the selecetd IPopUp prefab.

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
It contains a title and a description which can be set and modified in Runtime, a button that lets it change Canvas and a close button.
If it's no longer in the Camera's viewport, or if it is sufficiently obstructed, it will not be rendered.

#### RendererExtensions
This static class checks the prefab's partial or complete visibility

|   Functions   |   Type   |   Properties   |         |
|   ---------   |   ----   |   ----------   |   ---   |
|   **IsFullyVisibleFrom**   |   bool   |   RectTransform rectTransform, Camera camera   |   Checks if the given *rectTransform* corners and center are visible from the camera and returns true only if all of them are   |
|   **IsVisibleFrom**   |   bool   |   RectTransform rectTransform, Camera camera   |   Checks if the given *rectTransform* corners or center are visible from the camera and returns true if at least one is   |

#### PopUpInstance
Implements the IPopUp interface.
Manages the behaviour of the prefab. Allows Title and Description to be changed, as well as Canvas.

|   Properties   |   Type   |         |
|   ----------   |   ----   |   ---   |
|   **title**   |   string   |   value of the Title of the message. *Only available in the Editor*   |
|   **description**   |   string   |   value of the Title of the message. *Only available in the Editor*   |
|   **WSWidth**   |   float   |   width of the prefab while in the World Space Canvas. Measured in meters. *Only available in the Editor*   |
|   **SSWidth**   |   float   |   width of the prefab while in the Screen Space Canvas. Measured int pixels. *Only available in the Editor*   |
|   **parentCanvas**   |   enum CanvasStatus   |   defines in which canvas the prefab is at the start of the Runtime. *Only available in the Editor*   |
|   **titleObj**   |   TextMeshProUGUI   |   reference to the *TextMeshProUGUI* object that stores the title. *Only available in the Editor*   |
|   **descriptionObj**   |   TextMeshProUGUI   |   reference to the *TextMeshProUGUI* object that stores the description. *Only available in the Editor*   |
|   **mask**   |   Transform   |   used to stop the prefab from rendering when needed   |

|   Functions   |   Type   |   Properties   |         |
|   ---------   |   ----   |   ----------   |   ---   |
|   **ToggleLock**   |   void   |   None   |   Switches the prefab from Screen Space Canvas to World Space Canvas and viceversa   |
|   **SetLock**   |   void   |   bool locked   |   Depending on the value, sets the prefab's parent Canvas   |
|   **CheckPopUpVisibility**   |   void   |   None   |   Calls *RendererExtensions*' *IsVisibleFrom()* function to check for obstacles in the line of sight of the camera   |
|   **RemoveFromList**   |   void   |   None   |   Calls *PopUpManager* to remove the prefab from its list and Destroy it
|   **Die**   |   void   |   None   |   Called by *PopUpmanager*. Destroyes the GameObject
|   **SetTitle**   |   void   |   string title   |   changes the title of the message   |
|   **SetDescription**   |   void   |   string Description   |   changes the description of the message   |

#### PopUpManager
this static class keeps track and manages all alert icons and message popUps

|   Properties   |   Type   |         |
|   ----------   |   ----   |   ---   |
|   **CreatorList**   |   Dictionary<IPopUp, IPopUpCreator>   |   ReadOnly. Lists all pairs of popUps and icons   |
|   **PopUps**   |   List<IPopUp>   |   ReadOnly. Lists all PopUps present at RunTime   |

|   Functions   |   Type   |   Properties   |         |
|   ---------   |   ----   |   ----------   |   ---   |
|   **SetPopUpTitleAndScreen**   |   void   |   IPopUp obj, string title, string description   |   Changes the title and description of a given IPopUp object   |
|   **TryPairPopUp**   |   bool   |   IPopUp obj, IPopUpCreator creator   |   If *CreatorList* does not alrady contain the IPopUp object, add the new pair to the dictionary   |
|   **AddPopUp**   |   void   |   IPopUp newObj   |   Adds selected object to *PopUps* List   |
|   **DestroyPopUp**   |   void   |   IPopUp obj   |   If object is present in *PopUps*, removes the object and destroyes it   |
|   **CheckListIntegrity**   |   void   |   None   |   Checks *PopUps* List for null items and destroyes any it finds   |
|  **RequestWorldSpaceCanvas**  |   RectTransform   |   None   |   Returns the RectTransform of the World Space Canvas   |
|  **RequestScreenSpaceCanvas**  |   RectTransform   |   None   |   Returns the RectTransform of the Screen Space Canvas   |

