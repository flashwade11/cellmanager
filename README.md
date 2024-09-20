# cellmanager

A Package for managing cell type about color and markers while processing scRNA-seq data.

## Installation

It's easy to install cellmanager via pip!

```shell
pip install cellmanager
```

## Basic usage 

1. First, the user should prepare a TOML file as below.

   ```toml
   [Immnue]
   
   [Immnue.Lyphoid]
   
   [Immnue.Lyphoid.T]
   color = "#1ba169"
   markers = ["CD3D", "CD3E", "CD3G", "CD8A", "TRBC2"]
   
   [Immnue.Lyphoid.T."CD8+ T"]
   markers = ["CD8A", "CD8B"]
   
   [Immnue.Lyphoid.T."CD4+ T"]
   markers = ["CD40LG", "FOXP3", "CD4"]
   
   [Immnue.Lyphoid.NK]
   color = "#014431"
   markers = ["GNLY", "NKG7", "FGFBP2", "FCGR3A", "CX3CR1", "KLRB1", "NCR1"]
   ```

    Let me explain the basic logic of the data structure in the TOML file:

   - Top-level category (like [Immune] in the example above):
      The top-level category is indicated with square brackets []. In this case, [Immune] defines the main section of the file, grouping all immune-related data.

   - Nested subcategories [Immune.Lyphoid]:
      You can create nested sections by using dot notation (e.g., Lyphoid under Immune). This creates a hierarchical structure to organize related data within the top-level category.

   - Further sub-nesting [Immune.Lyphoid.T]:
      Further subcategories can be defined by continuing to nest sections using dots. For example, [Immune.Lyphoid.T] indicates a further refinement under Lyphoid.

   - Attributes:
     Inside each section, key-value pairs define attributes. 

     For most usage cases, `color` and `markers` are used frequently.

     color = "#1ba169" assigns a color attribute, and markers = [...] defines a list of markers.
   
   > [!TIP]
   >
   > We have designed the **#** character as a placeholder to keep the hierarchy consistent. We will introduce this below, so don't worry.
   


2. Create a CellManager object
   
   ```python
   from cellmanager import CellManager
   
   manager = CellManager("data.toml") # Input your TOML file path here.
   ```

3. Visualize your cell-type view

   ```python
   # tree 
   manager.render_tree()
   
   # level group
   
   manager.render_lavel_table()
   ```

   <img src="https://raw.githubusercontent.com/flashwade11/PicGo/master/PicGo/20240920150926.png" style="zoom:25%;" />![](https://raw.githubusercontent.com/flashwade11/PicGo/master/PicGo/20240920154438.png)

   > [!NOTE]
   >
   > The # character is used as a placeholder to keep related cell types at the same level.

   

4. Visualize the markers  of cell-type

   ```python
   # markers of all cell type
   manager.render_markers_table()
   
   # markers of specific cell type
   manager.render_markers_table(cluster="Endothelial")
   ```

   ![](https://raw.githubusercontent.com/flashwade11/PicGo/master/PicGo/20240920151113.png)

   ![](https://raw.githubusercontent.com/flashwade11/PicGo/master/PicGo/20240920151144.png)

5. Query information

   - `by` and `key` are the two most important parameters in `CellManager.query()`function.

   - `info` control to output `color` or  `markers`, default `markers`
   - `output_format` control to output as `dict` or `list`,  default `dict`.
   
   We provide three query methods:
   
   1. Query by cluster
   
      by = "cluster"
   
      key = `Which cluster do you want to know` (like "T" as below)
   
      include_major is a bool parameter controlling whether to output the major cell type.
   
      Example:
   
      ![](https://raw.githubusercontent.com/flashwade11/PicGo/master/PicGo/20240920151726.png)
   
   2. Query by level
   
      > [!WARNING]
      >
      > If you want to query by level, you must first run the `CellManager.render_level_table()`function to ensure that the user confirms the level information
   
      by = "level"
   
      key = `Which level do you want to know`
   
      Example:
   
      As shown above, there are eight cell types in the level 3.
   
      ![](https://raw.githubusercontent.com/flashwade11/PicGo/master/PicGo/20240920152604.png)
   
      ![](https://raw.githubusercontent.com/flashwade11/PicGo/master/PicGo/20240920154600.png)
   
   3. Query by a custom list
   
      Also, if you want to query a custom list you are interested in, you can use this method
   
      by = "list"
   
      key = [...]  everything you want to query 
   
      ![](https://raw.githubusercontent.com/flashwade11/PicGo/master/PicGo/20240920154649.png)
