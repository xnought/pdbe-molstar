# PDBe Molstar

PDBe Molstar is an interactive protein visualization library popularly used on [PDBe](https://www.ebi.ac.uk/pdbe/) and [AlphaFoldDB](https://alphafold.ebi.ac.uk/) to visualize protein structures. This library is an implementation of [Mol\* (/'mol-star/)](https://github.com/molstar/molstar).

<img width="500" alt="Demo of PDBeMolstar visualization." src="https://github.com/xnought/pdbe-molstar/assets/65095341/ede6bea0-ffc9-40a4-91b5-52b82443dffe">

See some of our **live examples** below with the JS Plugin `PDBeMolstarPlugin` or the web component `<pdbe-molstar>`:
| API  | Live Demo Link  | Description |
|---|---|---|
| JS Plugin  |  [Basic Usage](https://embed.plnkr.co/plunk/094fAnyWsuQVtYja) | Render a protein in using JavaScript with `PDBeMolstarPlugin`. |
| JS Plugin  |  [AlphaFold DB View](https://embed.plnkr.co/plunk/WlRx73uuGA9EJbpn) | Render an AlphaFold protein that contains pLDDT coloring with `alphafoldView: true`.|
| JS Plugin  |  [Helper functions](https://embed.plnkr.co/plunk/afXaDJsKj9UutcTD) | Programmatically interact with the protein via JavaScript with Helper Functions (e.g., custom coloring/selecting, rotating, etc.). |
| Web component  |  [Basic Usage](https://embed.plnkr.co/plunk/kKn7XBc8lZQ1GwKx) | Render a protein using the web component `<pdbe-molstar>`. |
| Web component  |  [Helper functions](https://embed.plnkr.co/plunk/m3GxFYx9cBjIanBp) | Programmatically interact with the protein from the web component. |
| Python Notebooks (external library) | [ipymolstar](https://github.com/Jhsmit/ipymolstar)| Wrapper on the `PDBeMolstarPlugin` for python notebooks by [Jhsmit](https://github.com/Jhsmit). This is an external library not affiliated with PDBe or Molstar.  |   |

**`README.md` Table of contents**
- Usage
  - [JS Plugin Usage](#js-plugin-usage): use `PDBeMolstarPlugin` to render proteins in JavaScript
  - [Web Component Usage](#web-component-usage): use `<pdbe-molstar>` to render proteins with just HTML
- API Reference
  - [JS Plugin API Reference](#js-plugin-api-reference): render options 
  - [Web Component API Reference](#web-component-api-reference): HTML attributes (mirrors render options in JS Plugin API)
  - [Helper Methods API Reference](#helper-methods-api-reference): ways to interact with the protein programmatically (e.g., toggle spin, toggle full screen, custom coloring, etc.).

## JS Plugin Usage

**1. Include the style and script files of the library in your web page**

```html
<!-- CSS -->
<link rel="stylesheet" type="text/css" href="https://www.ebi.ac.uk/pdbe/pdb-component-library/css/pdbe-molstar-3.1.3.css">

<!-- For Light Theme include this css file instead of the above -->
<!--<link rel="stylesheet" type="text/css" href="https://www.ebi.ac.uk/pdbe/pdb-component-library/css/pdbe-molstar-light-3.1.3.css">-->
<!--
Tip: Set `bgColor` render option to white ({r:255,g:255,b:255}) for light theme in js instance initialization
-->

<!-- JS -->
<script type="text/javascript" src="https://www.ebi.ac.uk/pdbe/pdb-component-library/js/pdbe-molstar-plugin-3.1.3.js"></script>
```

**2. Instantiate plugin and provide parameters (options) to render**
```html
<script>
    /**
     * Initialize JS Plugin
     */
    const viewerInstance = new PDBeMolstarPlugin(); // JS Plugin
    const viewerContainer = document.getElementById('myViewer'); // in DOM

    /**
     * Render protein from PDBe with id: 1cbs
     */
    const options = {
	    moleculeId: '1cbs' 
    } // ❗️See API Reference for more render options
    viewerInstance.render(viewerContainer, options);
</script>
```

**Note: you can easily change the render options to include custom protein files:**

```html
<script>
    /**
     * Initialize JS Plugin
     */
    const viewerInstance = new PDBeMolstarPlugin(); // JS Plugin
    const viewerContainer = document.getElementById('myViewer'); // in DOM

    /**
     * Render protein from .cif file https://alphafold.ebi.ac.uk/files/AF-O15552-F1-model_v1.cif
     */
    const options = {
        customData: {
          url: 'https://alphafold.ebi.ac.uk/files/AF-O15552-F1-model_v1.cif',
          format: 'cif'
        },
      } // ❗️See API Reference for more render options
    viewerInstance.render(viewerContainer, options);
</script>
```
Example above from [AlphaFold DB View](https://embed.plnkr.co/plunk/WlRx73uuGA9EJbpn) live demo.

See the [render options](#viewerinstancerenderviewercontainer-renderoptions) in the API reference for more options.

## Web Component Usage

**1. Include the style and script files of the library in your web page**

```html
<!-- Required for IE11 -->
<script src="https://cdn.jsdelivr.net/npm/babel-polyfill/dist/polyfill.min.js"></script>
<!-- Web component polyfill (only loads what it needs) -->
<script src="https://cdn.jsdelivr.net/npm/@webcomponents/webcomponentsjs/webcomponents-lite.js" charset="utf-8"></script>
<!-- Required to polyfill modern browsers as code is ES5 for IE... -->
<script src="https://cdn.jsdelivr.net/npm/@webcomponents/webcomponentsjs/custom-elements-es5-adapter.js" charset="utf-8"></script>

<!-- CSS -->
<link rel="stylesheet" type="text/css" href="https://www.ebi.ac.uk/pdbe/pdb-component-library/css/pdbe-molstar-3.1.3.css">

<!-- For Light Theme include this css file instead of above -->
<!--<link rel="stylesheet" type="text/css" href="https://www.ebi.ac.uk/pdbe/pdb-component-library/css/pdbe-molstar-light-3.1.3.css">-->
<!--
Tip: Set background color to white for light theme in component initialization
-->

<!-- JS -->
<script type="text/javascript" src="https://www.ebi.ac.uk/pdbe/pdb-component-library/js/pdbe-molstar-component-3.1.3.js"></script>
```
*Until web components are natively supported by all browsers, it is necessary to use polyfills

**2. Include PDBe Molstar as HTML Element**
```html
<!-- ❗️See API Reference for more render options -->
<pdbe-molstar molecule-id="1cbs"></pdbe-molstar>
```

**Note you can add custom data easily like:**

```html
<pdbe-molstar 
  custom-data-url="https://alphafold.ebi.ac.uk/files/AF-O15552-F1-model_v1.cif"
  custom-data-format="cif"
></pdbe-molstar>
```

For more initialization options, check the [Web Component API Reference](#web-component-api-reference).


## JS Plugin API Reference

Given that  we have a `viewerInstance` and `viewerContainer` initialized

```html
<script>
    const viewerInstance = new PDBeMolstarPlugin(); // JS Plugin
    const viewerContainer = document.getElementById('myViewer'); // in DOM
</script>
```

### viewerInstance.`render`(viewerContainer, renderOptions)

- `viewerInstance` is a `PDBeMolstarPlugin` object
- `viewerContainer` must be a DOM element (container for the visuals)
- `renderOptions` an object with the options specified below


Value for either `moleculeId` or `customData` is required (if both are provided then `customData` is preferred). All other options are optional. If you prefer the Typescript interface, you can also look to [`spec.ts`](./src/app/spec.ts#L68).

|Option|Value type|Details|
|---|---|---|
|moleculeId|`string`|PDB ID - Example: '1cbs' <br>(or UniProt ID if `superposition` is `true`)|
|customData|`{ url: string, format: string, binary?: boolean}`|Load data from a specific data source<br> Example:  `{ url: 'https://www.ebi.ac.uk/pdbe/model-server/v1/1cbs/atoms?label_entity_id=1&auth_asym_id=A&encoding=bcif', format: 'cif', binary:true }` |
|assemblyId|`string`|Display specific assembly of an entry<br>Example: '1'<br>*** Use value 'preferred' to render the default assembly (i.e. the first assembly). Leave undefined to load deposited model structure.|
|defaultPreset|`string`|Set the type of structure to be loaded. Expected value is `'default', 'unitcell', 'all-models', 'supercell'`|
|ligandView|`{ auth_asym_id?: string, struct_asym_id?: string, label_comp_id?: string, auth_seq_id?: number, show_all?: boolean }`|This option can be used to display the PDBe ligand page 3D view like [here (REA)](https://www.ebi.ac.uk/pdbe/entry/pdb/1cbs/bound/REA).<br>Example: `{ label_comp_id: 'REA' }`<br>*** At least one of `label_comp_id` and `auth_seq_id` is required.|
|alphafoldView|`boolean`<br>Default `false`|This applies AlphaFold confidence score coloring theme for AlphaFold model|
|superposition|`boolean`<br>Default `false`|Display the superposed structures view like the one on the PDBe-KB pages|
|superpositionParams|`{ matrixAccession?: string, segment?: number, cluster?: number[], superposeCompleteCluster?: boolean, ligandView?: boolean, superposeAll?: boolean, ligandColor?: { r: number, g: number, b: number } }`|Customize the superposed structures view. Example: `{ matrixAccession: 'P08684', segment: 1, ligandView: true, ligandColor: { r: 255, g: 255, b: 50 } }`|
|selection|`{ data: QueryParam[], nonSelectedColor?: any, clearPrevious?: boolean }`<br>where [QueryParam](https://github.com/molstar/pdbe-molstar/blob/master/src/app/helpers.ts#L170)|Specify parts of the structure to highlight with different colors|
|visualStyle|`string`|Set default visual style.<br>Accepted values: `'cartoon' \| 'ball-and-stick' \| 'carbohydrate' \| 'ellipsoid' \| 'gaussian-surface' \| 'molecular-surface' \| 'point' \| 'putty' \| 'spacefill'`<br>Leave undefined to use default visual styles for each component type (polymer, ligand etc.)|
|hideStructure|`string[]`|Molstar renders multiple visuals (polymer, ligand, water...) visuals by default. This option is to exclude any of these default visuals. Expected value is an array with `'polymer', 'het', 'water', 'carbs', 'nonStandard', 'coarse'` keywords. For example `hideStructure: ['water']` will not render water visual in the 3D view|
|loadMaps|`boolean`<br>Default `false`|Load electron density (or EM) maps from the Volume Server if value is set to true|
|mapSettings|`{ 'em'?: MapStyle, '2fo-fc'?: MapStyle, 'fo-fc(+ve)'?: MapStyle, 'fo-fc(-ve)'?: MapStyle }`<br>where `MapStyle = { opacity?: number, wireframe?: boolean }`|Customize map style (opacity and solid/wireframe). Only applicable when `loadMaps` is set to `true`.<br>Example: `{ em: { opacity: 0.4, wireframe: false } }`|
|bgColor|`{r:number, g:number, b:number}`<br>Default `{r:0, g:0, b:0}`|Set canvas background color.<br> Example: `{r:255, g:255, b:255}`|
|highlightColor|`{r:number, g:number, b:number}`<br>Default `{r:255, g:102, b:153}`|Set default color appearing on mouse-over.<br>Example: `{r:255, g:255, b:102}`|
|selectColor|`{r:number, g:number, b:number}`<br>Default `{r:51, g:255, b:25}`|Set default color for marking the selected part of structure (when Selection Mode is active).<br>Example: `{r:255, g:255, b:102}`|
|lighting|`string`|Set default lighting. Expected value is `'flat', 'matte', 'glossy', 'metallic', 'plastic'`|
|validationAnnotation|`boolean`<br>Default `false`|Load Validation Report Annotations. Adds 'Annotations' control in the menu|
|domainAnnotation|`boolean`<br>Default `false`|Load Domain Annotations. Adds 'Annotations' control in the menu|
|symmetryAnnotation|`boolean`<br>Default `false`|Load Assembly Symmetry Annotations. Adds 'Annotations' control in the menu|
|pdbeUrl|`string`<br>Default `https://www.ebi.ac.uk/pdbe/`|This option is to set the default base URL for the data source. Mostly used internally to test the plugin on different environments|
|encoding|`string`<br>Default `'bcif'`|Preferred encoding of input structural data.<br> Accepted values: `'bcif' \| 'cif'`|
|lowPrecisionCoords|`boolean`<br>Default `false`|Load low precision coordinates from Model Server|
|selectInteraction|`boolean`<br>Default `true`|Controls the action performed when clicking a residue. `true` (default) will zoom the residue and show ball-and-stick visual for its surroundings, `false` will only zoom the residue. If `ligandView` or `superposition` option is set, `selectInteraction` behaves as if `false`.|
|selectBindings|`object` [DefaultSelectLociBindings](https://github.com/molstar/molstar/blob/master/src/mol-plugin/behavior/dynamic/representation.ts#L96)|Override mouse selection behavior|
|focusBindings|`object` [DefaultFocusLociBindings](https://github.com/molstar/molstar/blob/master/src/mol-plugin/behavior/dynamic/camera.ts#L39)|Override mouse click focus behavior|
|granularity|`string`<br>Default `'residue'`|Structure granularity level for interactions like highlight, focus, select.<br> Accepted values: `'element' \| 'residue' \| 'chain' \| 'entity' \| 'model' \| 'operator' \| 'structure' \| 'elementInstances' \| 'residueInstances' \| 'chainInstances'`. (Granularity levels ending with `Instances` treat multiple copies of the same element/residue/chain in an assembly as one object).|
|subscribeEvents|`boolean`<br>Default `false`|Subscribe to other PDB Web-components custom events|
|hideControls|`boolean`<br>Default `false`|Hide all control panels by default (can be shown by the Toggle Controls Panel button (wrench icon))|
|hideCanvasControls|`string[]`|Hide individual icon buttons in the top-right corner of the canvas. Expected value is an array with `'expand', 'selection', 'animation', 'controlToggle', 'controlInfo'` keywords.|
|sequencePanel|`boolean`<br>Default `false`|Display Sequence panel|
|pdbeLink|`boolean`<br>Default `true`|Display PDBe entry link in top right corner of the canvas|
|loadingOverlay|`boolean`<br>Default `false`|Show overlay with PDBe logo while the initial structure is being loaded|
|expanded|`boolean`<br>Default `false`|Display full-screen by default on load|
|landscape|`boolean`<br>Default `false`|Set landscape view (control panels on the sides instead of above and under the canvas)|
|reactive|`boolean`<br>Default `false`|Set reactive layout (switching between landscape and portrait based on the browser window size). Overrides `landscape`.|

## Web Component API Reference

### `<pdbe-molstar></pdbe-molstar>` attributes

These attributes match the render initialization options in [JS Plugin API Reference](#viewerinstancerenderviewercontainer-renderoptions), but for the HTML component `<pdbe-molstar></pdbe-molstar>`.

|Attribute|Details|
|---|---|
|`molecule-id`|Load PDB Data<br>Example:<br>`<pdbe-molstar molecule-id="1cbs"></pdbe-molstar>`|
|`custom-data-url`<br>`custom-data-format`<br>`custom-data-binary`|Load Custom Data<br>Example:<br>`<pdbe-molstar custom-data-url="https://www.ebi.ac.uk/pdbe/model-server/v1/1cbs/full" custom-data-format="cif"></pdbe-molstar>`<br>**`custom-data-binary` is optional|
|`assembly-id`|Specify Assembly<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" assembly-id="1"></pdbe-molstar>`|
|`default-preset`|Set default Preset view<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" default-preset="unitcell"></pdbe-molstar>`<br><br>***Accepted values - `'default' \| 'unitcell' \| 'all-models' \| 'supercell'`|
|`ligand-label-comp-id`<br>`ligand-auth-asym-id`<br>`ligand-struct-asym-id`<br>`ligand-auth-seq-id`<br>`ligand-show-all`|Load PDBe Ligand view<br>[Example (REA)](https://www.ebi.ac.uk/pdbe/entry/pdb/1cbs/bound/REA)<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" ligand-label-comp-id="REA"></pdbe-molstar>`<br><br>*** `molecule-id` is required.<br> At least one of `ligand-label-comp-id` and `ligand-auth-seq-id` is required.|
|`alphafold-view`|This applies AlphaFold confidence score colouring theme for AlphaFold model<br>**default - false|
|`visual-style`|Visual styling<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" visual-style="spacefill"></pdbe-molstar>`<br><br>***Accepted values - `'cartoon' \| 'ball-and-stick' \| 'carbohydrate' \| 'distance-restraint' \| 'ellipsoid' \| 'gaussian-surface' \| molecular-surface' \| 'point' \| 'putty' \| 'spacefill'`|
|`hide-polymer`<br>`hide-water`<br>`hide-het`<br>`hide-carbs`<br>`hide-non-standard`<br>`hide-coarse`|Hide visuals<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" hide-water="true" hide-het="true"></pdbe-molstar>`<br><br>***will not render HET and water visual in the 3D view|
|`load-maps`|Load Electron Density Maps from the Volume Server<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" load-maps="true"></pdbe-molstar>`<br><br>**default - false|
|`bg-color-r`<br>`bg-color-g`<br>`bg-color-b`|Background color<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" bg-color-r="255" bg-color-g="255" bg-color-b="255"></pdbe-molstar>`<br><br>This will load structure for '1cbs' and set background color to white|
|`highlight-color-r`<br>`highlight-color-g`<br>`highlight-color-b`|Highlight color<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" highlight-color-r="255" highlight-color-g="102" highlight-color-b="153"></pdbe-molstar>`<br><br>This will set the default color appearing on mouse-over|
|`select-color-r`<br>`select-color-g`<br>`select-color-b`|Selection color<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" select-color-r="255" select-color-g="102" select-color-b="153"></pdbe-molstar>`<br><br>This will set the default selection appearing on Shift key + mouse-click|
|`lighting`|Set default lighting<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" lighting="plastic"></pdbe-molstar>`<br><br>***Accepted values - `'flat' \| 'matte' \| 'glossy' \| 'metallic' \| 'plastic'`|
|`validation-annotation`|Load Validation Report Annotation<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" validation-annotation="true"></pdbe-molstar>`<br><br>This will add 'Annotations' control in the menu. **default - false|
|`domain-annotation`|Load Domain Annotation<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" domain-annotation="true"></pdbe-molstar>`<br><br>This will add 'Annotations' control in the menu. **default - false|
|`symmetry-annotation`|Load Assembly Symmetry Annotation<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" symmetry-annotation="true"></pdbe-molstar>`<br><br>This will add 'Annotations' control in the menu. **default - false|
|`pdbe-url`|Url for PDB data<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" pdbe-url="https://www.ebi.ac.uk/pdbe/"></pdbe-molstar>`<br><br>***This option is to set the default base url for the data source. Mostly used internally to test the plugin on different environments|
|`encoding`|Preferred encoding of input structural data<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" encoding="cif"></pdbe-molstar>`<br><br>**default - `"bcif"`|
|`low-precision`|Load low precision coordinates from the Model server<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" low-precision="true"></pdbe-molstar>`<br><br>**default - true|
|`select-interaction`|Switch off the default selection interaction behaviour. This option is used while rendering the 'ligandView'<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" select-interaction="false"></pdbe-molstar>`<br><br>**default - true|
|`subscribe-events`|Subscribe to other PDB Web-components custom events<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" subscribe-events="true"></pdbe-molstar>`<br><br>**default - false|
|`hide-controls`|Hide controls menu<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" hide-controls="true"></pdbe-molstar>`<br><br>**default - false|
|`hide-expand-icon`<br>`hide-selection-icon`<br>`hide-animation-icon`|Hide canvas controls<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" hide-expand-icon="true"></pdbe-molstar>`<br><br>This will hide the expand / fullscreen icon|
|`sequence-panel`|Display Sequence panel|
|`pdbe-link`|Display PDBe entry link in top right corner of the canvas<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" pdbe-link="false"></pdbe-molstar>`<br><br>**default - true|
|`expanded`|Display full-screen by default on load<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" expanded="true"></pdbe-molstar>`<br><br>**default - false|
|`landscape`|Set landscape layout (control panels on the sides instead of above and under the canvas). The controls will similar to the full-screen view.<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" landscape="true"></pdbe-molstar>`<br><br>**default - false|
|`reactive`|Set reactive layout (switching between landscape and portrait based on the browser window size).<br>Example:<br>`<pdbe-molstar molecule-id="1cbs" reactive="true"></pdbe-molstar>`<br><br>**default - false|

## Helper Methods API Reference

The `viewerInstance` (the PDBeMolstarPlugin object) has many helper functions for you to programmatically interact with the protein visuals.

To remind you, you get the `viewerInstance` with the [JS Plugin Usage](#js-plugin-usage) when you initialize the `PDBeMolstarPlugin` like

```html
<script>
    const viewerInstance = new PDBeMolstarPlugin(); // JS Plugin
    const viewerContainer = document.getElementById('myViewer'); // in DOM
</script>
```

or you can retrieve it from the [Web Component Usage](#web-component-usage) like

```html
<!-- Molstar container -->
<pdbe-molstar id="pdbeMolstarComponent" molecule-id="2nnu" hide-controls="true"></pdbe-molstar>
<script>
  let viewerInstance;
  window.onload = function () {
      const pdbeMolstarComponent = document.getElementById('pdbeMolstarComponent'); // in DOM
      viewerInstance = pdbeMolstarComponent.viewerInstance; //❗:type PDBeMolstarPlugin
  };
</script>
```

### Canvas/layout methods
|No.|Function|Parameters|Description|
|---|---|---|---|
|1|setBgColor|`color`<br>Type: `json`<br>`{r:number, g:number, b:number}`|Set Canvas background color<br>Example:`viewerInstance.canvas.setBgColor({r:255, g:255, b:255})` <br>will set the background to white
|2|toggleControls|`isVisible`<br>Type: `boolean`<br>`true\|false`<br><i>Optional</i>|Toggle controls menu<br>Example:`viewerInstance.canvas.toggleControls(true)`<br>will make the controls visible|
|3|toggleExpanded|`isExpanded`<br>Type: `boolean`<br>`true\|false`<br><i>Optional</i>|Toggle full-screen<br>Example:`viewerInstance.canvas.toggleExpanded(true)`<br>will switch to full-screen|

### Visual methods
|No.|Function|Parameters|Description|
|---|---|---|---|
|1|visibility|`data`<br>Type: `json`<br>`{polymer: boolean, het: boolean, water: boolean, carbs: boolean, maps: boolean}`|Change the visual entities visibility<br>Example:`viewerInstance.visual.visibility({het:false, water:false})` <br>will hide the HET and water visuals|
|2|toggleSpin|`isSpinning`<br>Type: `boolean`<br>`true\|false`<br><i>Optional</i>|Toggle visual rotation<br>Example:`viewerInstance.visual.toggleSpin(true)`<br>will rotate the visual|
|3|focus|`params`<br>Type: `json`<br>`[{entity_id?: string, auth_asym_id?: string, struct_asym_id?: string, residue_number?: number, start_residue_number?: number, end_residue_number?: number, auth_residue_number?: number, auth_ins_code_id?: string, start_auth_residue_number?: number, start_auth_ins_code_id?: string, end_auth_residue_number?: number, end_auth_ins_code_id?: string, atoms?: string[], label_comp_id?: string}[], atom_id?: number[]}], uniprot_accession?: string, uniprot_residue_number?: number, start_uniprot_residue_number?: number, end_uniprot_residue_number?: number, structureNumber?: number`|Focus on a particular visual section <br>Example:`viewerInstance.visual.focus([{entity_id: '1', struct_asym_id: 'A', start_residue_number: 10, end_residue_number: 15}])`<br>will focus on residue range 10-15 of Chain 'A' of Entity 1|
|4|highlight|`params`<br>Type: `json`<br>`{ data: [{entity_id?: string, auth_asym_id?: string, struct_asym_id?: string, residue_number?: number, start_residue_number?: number, end_residue_number?: number, auth_residue_number?: number, auth_ins_code_id?: string, start_auth_residue_number?: number, start_auth_ins_code_id?: string, end_auth_residue_number?: number, end_auth_ins_code_id?: string, atoms?: string[], label_comp_id?: string, atom_id?: number[]}], color: {r:number, g:number, b:number}, focus?: boolean, uniprot_accession?: string, uniprot_residue_number?: number, start_uniprot_residue_number?: number, end_uniprot_residue_number?: number, structureNumber?: number }`|Trigger highlight<br>Example:`viewerInstance.visual.highlight({ data: [{entity_id: '1', struct_asym_id: 'A', start_residue_number: 10, end_residue_number: 15}] })`|
|5|clearHighlight|-|Clear highlight<br>Example:`viewerInstance.visual.clearHighlight()`|
|6|select|`params`<br>Type: `json`<br>`{ data: [{entity_id?: string, auth_asym_id?: string, struct_asym_id?: string, residue_number?: number, start_residue_number?: number, end_residue_number?: number, auth_residue_number?: number, auth_ins_code_id?: string, start_auth_residue_number?: number, start_auth_ins_code_id?: string, end_auth_residue_number?: number, end_auth_ins_code_id?: string, atoms?: string[], label_comp_id?: string, atom_id?: number[], color: {r:number, g:number, b:number}, focus?: boolean, sideChain?: boolean, representation?: 'cartoon\|backbone\|ball-and-stick\|ellipsoid\|gaussian-surface\|line\|molecular-surface\|spacefill', representationColor?: {r:number, g:number, b:number}, uniprot_accession?: string, uniprot_residue_number?: number, start_uniprot_residue_number?: number, end_uniprot_residue_number?: number}], nonSelectedColor: {r:number, g:number, b:number}, structureNumber?: number }`|Color and focus on a specific visual section<br>Example:`viewerInstance.visual.select({ data: [{entity_id: '1', struct_asym_id: 'A', start_residue_number: 10, end_residue_number: 15, color:{r:255,g:0,b:0}, focus: true}], nonSelectedColor: {r:255,g:255,b:255} })`<br>will color residue range 10-15 of Chain 'A' of Entity 1 in red and other residues with the provided default yellow color.|
|7|clearSelection|Optional: `structureNumber`<br>Type: `number`|Clear Selection<br>Example:`viewerInstance.visual.clearSelection()`|
|8|setColor|`data`<br>Type: `json`<br>`{ highlight?: {r:number, g:number, b:number}, select?: {r:number, g:number, b:number} }`|Set highlight / selection colour<br>Example:`viewerInstance.visual.setColor({ highlight: {r:255,g:255,b:0} })`<br> will set highlight colour to yellow. The selection colour refers to the colour used in the Selection Mode (the mouse cursor icon) and is not related to the colour used by the `select` method above.|
|9|reset|`data`<br>Type: `json`<br>`{ camera?: boolean, theme?: boolean, highlightColor?: boolean, selectColor?: boolean }`|Reset to defaults<br>Example:`viewerInstance.visual.reset({ camera: true, theme: true })`<br>will reset camera and theme to defaults|
|10|update|(options: PluginParams, reLoad?: boolean)<br> for plugin params [Refer here](https://github.com/PDBeurope/pdbe-molstar/wiki/1.-PDBe-Molstar-as-JS-plugin#plugin-parameters-options)|Updates the instance<br>Example:`viewerInstance.visual.update({moleculeId: '1cbs'})`<br>This method particularly used to update the main data(source)<br><br>Set reload param to false to load additional structure<br>Example:`viewerInstance.visual.update({moleculeId: '1cbs'}, false)`|

### Custom Events
Event listeners can be used to bind the PDBe Molstar custom events

|No.|Event|Description|
|---|---|---|
|1|PDB.molstar.click|Binds to click event. Event data (available in key = 'eventData') contains information structure residue clicked<br>Example:`document.addEventListener('PDB.molstar.click', (e) => { //do something on event });`|  
|2|PDB.molstar.mouseover|Binds to mouseover event.<br>Example:`document.addEventListener('PDB.molstar.mouseover', (e) => { //do something on event });`| 
|3|PDB.molstar.mouseout|Binds to mouseout event.<br>Example:`document.addEventListener('PDB.molstar.mouseout', () => { //do something on event });`|
|4|loadComplete|This is a reactive event available on the viewer instance. It is fired on load/render completion.<br>Example:`viewerInstance.events.loadComplete.subscribe(() => { //do something after 3d view is rendered });`|



## Development

### Building & Running locally

```sh
npm install
npm run build
# npm run rebuild  # for a clean build
npm run serve
```

### Build automatically on file save:

```sh
npm run watch
```

### Manual testing

- Run locally by `npm run serve`
- Go to <http://127.0.0.1:1338/portfolio.html> and check the viewer with various different setting (some of these reflect the actual setting on PDBe pages)
- If you want to tweak the options, go to "Frame URL" and change the options in the URL

### Deployment

- Bump version in `package.json` using semantic versioning
  - Use a version number like "1.2.3-beta.1" for development versions (to be used in development environment wwwdev.ebi.ac.uk)
  - Use a version number like "1.2.3" for proper releases
- Ensure install, lint, and rebuild works locally
- Update `CHANGELOG.md`
- Git commit and push
- Create a git tag matching the version with prepended "v" (e.g. "v1.2.3")
- The GitHub repo will automatically be mirrored to EBI GitLab (might take up to 1 hour)
- CICD pipeline in EBI GitLab will automatically publish the package to npm (https://www.npmjs.com/package/pdbe-molstar)
- The files will become available via JSDeliver
  - https://cdn.jsdelivr.net/npm/pdbe-molstar@dev/build/pdbe-molstar-plugin.js for the latest version including development versions
  - https://cdn.jsdelivr.net/npm/pdbe-molstar@latest/build/pdbe-molstar-plugin.js for the latest proper release
