## Grid
  
### Spawning a Tile Grid

Example for creating a grid by spawning tiles


#### Steps

* Set the Pivot Point of your Tile Sprite to Top Left
* Create a Tile Prefab with
 * Sprite Renderer
 * Box Collider 2D
 * Tile (Script) attached
* Create an empty GameObject ("Tiles" or something) that acts as a parent for the tiles (to not clutter the Hierarchy)
* Put the TileSpawner at a GameObject (for example on "Management")
* Set the references / values in the TileSpawner Component


###### TileSpawner.cs ####

```csharp
using UnityEngine;
using System.Collections;

public class TileSpawner : MonoBehaviour {

    [SerializeField]
    GameObject tilePrefab;

    [SerializeField]
    Transform tilesContainer;

    [SerializeField]
    int gridWidth;

    [SerializeField]
    int gridHeight;

  // TODO: Check references in Awake

    void Start ()
    {
        Vector3 worldStartPosition = Camera.main.ScreenToWorldPoint(new Vector3(0, Camera.main.pixelHeight, Camera.main.nearClipPlane));

        for (int x = 0; x < gridWidth; x++)
        {
            for (int y = 0; y < gridHeight; y++)
            {
                float tileXPos = worldStartPosition.x + tilePrefab.GetComponent<SpriteRenderer>().bounds.size.x * x;
                float tileYPos = worldStartPosition.y - tilePrefab.GetComponent<SpriteRenderer>().bounds.size.y * y;
                Vector3 tilePosition = new Vector3(tileXPos, tileYPos, 0);
                GameObject clone = Instantiate(tilePrefab, tilePosition, Quaternion.identity, tilesContainer) as GameObject;
                clone.GetComponent<Tile>().posX = x + 1;
                clone.GetComponent<Tile>().posY = y + 1;
            }
        }
    }

}
```
  
###### Tile.cs ####

```csharp
using UnityEngine;
using System.Collections;

public class Tile : MonoBehaviour {

    public int posX;
    public int posY;


    void OnMouseDown()
    {
        Debug.Log("Mouse down on tile at Grid Position: (" + posX + ", "+ posY + ")");
    }

}
```
