#Multi-Node Tree Picker (MNTP)

`Returns: XML or CSV`

The multi-node tree picker data type allows your content editor to choose multiple nodes in the content or media trees to be saved with the current document type. This is useful for all sorts of situations such as relating a page to numerous other pages, creating a list of images/files from the media section, etc...

##Data Type Definition Example

![Media Picker Data Type Definition](images/MNTP-DataType.jpg)

##Settings

There are a lot of settings for the Multi-Node tree picker, there are also some settings which determine which further settings are displayed.

###Select tree type

This setting determines if the data type will allow users to pick from either content or media. Changing this setting determines further settings in the property editor as detailed below.

![Media Picker Data Type Definition](images/MNTP-Settings-SelectTree.jpg)

###Node selection type 
*This setting is only available when "Select tree type" setting is set to "Content".*

This setting determines if the tree picker nodes are selected by a particular parent node or by using a XPath expression. **NOTE: If using XPath only published nodes are shown.**

![Media Picker Data Type Definition](images/MNTP-Settings-NodeSelectionType.jpg) 

###XPath Type

*This setting is only available when "Select tree type" setting is set to "Content" and "Node selection type" is set to "XPath Expression".*

This setting determines if the XPath expression is evaluated from the node being currently edited or from the root of the global tree.

When "XPath Type" is set to "Global", the expression is evaluated by using [uQuery.GetNodesByXPath](../../../../Reference/Querying/uQuery/Content/Nodes.md) This method provides some very useful tokens such as $ancestorOrSelf which allow complex expressions.

![Media Picker Data Type Definition](images/MNTP-Settings-XPathType.jpg) 

###XPath expression
*This setting is only available when "Select tree type" setting is set to "Content" and "Node selection type" is set to "XPath Expression".*

The XPath expression should select the starting node for the editor to choose from. The expression depends on the "XPath Type" setting to determine if it is evaluated against the node being edited or the global tree.

Some common examples:

`self::*` - used with "XPath Type" being set to "From Current" - this would allow the editor to select from descendants of the node being edited

`$ancestorOrSelf/ancestor-or-self::*[@isDoc][@level=1]` - used with "XPath Type" being set to "Global" - this would allow the editor to select descendants of the homepage (assuming at level 1). See "XPath Type" setting above for more information on using tokens such as $ancestorOrSelf

![Media Picker Data Type Definition](images/MNTP-Settings-XPathExpression.jpg) 

###Start node ID

*This setting is only available when "Select tree type" setting is set to "Content" and "Node selection type" is set to "Node Picker" or "Select tree type" is set to "Media".*

Use a content or media picker (depending on the "Node selection type" setting) to set the starting node for the editor to choose from.

![Media Picker Data Type Definition](images/MNTP-Settings-StartNodeID.jpg) 

###Show thumbnails for media items?

*This setting is only available when "Select tree type" setting is set to "Media".*

When checked, the editor will see a thumbnail preview of the media item in the selected items.

![Media Picker Data Type Definition](images/MNTP-Settings-Thumbs.jpg) 

###XPath filter type

The "XPath filter type" determines if the "XPath filter" expression allows the editor to select matched nodes (enabled) or  doesn't allow the editor to select matched nodes (disabled).

![Media Picker Data Type Definition](images/MNTP-Settings-XPathFilterType.jpg) 

###XPath filter

The "XPath filter" is an XPath expression either allow or disallow (depending on "XPath filter type" setting above) the selection of matched nodes.  

![Media Picker Data Type Definition](images/MNTP-Settings-XPathFilter.jpg) 

###Maximum node selections

Sets the maximum number of nodes the editor can select. Use -1 for unlimited.

![Media Picker Data Type Definition](images/MNTP-Settings-MaxNodes.jpg) 

###Minimum node selections

Sets the minimum number of nodes the editor can select.

![Media Picker Data Type Definition](images/MNTP-Settings-MinNodes.jpg) 

###Show tooltip for selected item

If checked a information icon will appear next to selected nodes, this provides further information about the node to the editor.

![Media Picker Data Type Definition](images/MNTP-Settings-Tooltip.jpg) 

###Data as CSV or XML?

This setting determines if the data is stored as CSV data or XML.

![Media Picker Data Type Definition](images/MNTP-Settings-CSVXML.jpg) 

Example of XML data:
  
	<MultiNodePicker>
		<nodeId>1052</nodeId>
		<nodeId>1051</nodeId>
		<nodeId>1050</nodeId>
	</MultiNodePicker>

###Pixel height of the tree control box

This setting determines the height of the data type when displayed to the editor.

![Media Picker Data Type Definition](images/MNTP-Settings-Height.jpg)

##Content Example 

The list on the right shows which nodes have already been selected/saved

These nodes can be sorted by dragging and dropping and can also be removed by clicking the 'x'

![Media Picker Content Example 1](images/MNTP-Content-Standard.jpg)

When informational tooltips are enabled an 'info' icon is displayed next to each of the selected items 
which when click shows information about the node:

- The node's ID
- Path and content Path
- Edit button which will deep link to that item's editor page

![Media Picker Content Example 2](images/MNTP-Content-Standard-Info.jpg)

The nodes in a light reddish colour ("File 1") are nodes that the user is unable to select

![Media Picker Content Example 3](images/MNTP-Content-Media.jpg)

##MVC View Example

###Typed:

For XML data storage (DynamicXml used to retrieve NodeIds):

	@using Umbraco.Core.Dynamics;
	@{
	    if (Model.Content.HasValue("mntpFeaturePickerXML"))
	    {
	        var typedMultiNodeTreePicker = new DynamicXml(Model.Content.GetPropertyValue<string>("mntpFeaturePickerXML"));
	        var typedPublishedMNTPNodeList = new List<string>();
	        foreach (dynamic id in typedMultiNodeTreePicker)                
	        {
	            typedPublishedMNTPNodeList.Add(id.InnerText); 
	        }
	        var typedMNTPCollection = Umbraco.TypedContent(typedPublishedMNTPNodeList).Where(x => x != null);
	        foreach (var item in typedMNTPCollection)
	        {     
	            <p>@item.Name</p>         
	        }       
	    }
	}

For CSV data storage:
	
	@{
	    if (Model.Content.HasValue("mntpFeaturePickerCSV"))
	    {
	        var typedMultiNodeTreePickerCSV = Model.Content.GetPropertyValue<string>("mntpFeaturePickerCSV");
	        var typedPublishedMNTPNodeListCSV = typedMultiNodeTreePickerCSV.Split(new string[] { "," }, StringSplitOptions.RemoveEmptyEntries).Select(int.Parse);
	        var typedMNTPCollectionCSV = Umbraco.TypedContent(typedPublishedMNTPNodeListCSV).Where(x => x != null);
	        foreach (var item in typedMNTPCollectionCSV)
	        {     
	            <p>@item.Name</p>         
	        }   
	    }    
	}


###Dynamic: 

For XML data storage:

	@{
	    var dynamicPublishedMNTPNodeList = new List<string>();
	    foreach (var id in CurrentPage.mntpFeaturePickerXML)                
	    {
	        dynamicPublishedMNTPNodeList.Add(id.InnerText); 
	    }
	    var dynamicMNTPCollection = Umbraco.Content(dynamicPublishedMNTPNodeList);
	    foreach (var item in dynamicMNTPCollection)
	    {     
	        <p>@item.Name</p>         
	    }       
	}  

For CSV data storage:

	@{
	    var dynamicPublishedMNTPNodeListCSV = CurrentPage.mntpFeaturePickerCSV.Split(new string[] { "," }, StringSplitOptions.RemoveEmptyEntries);
	    var dynamicMNTPCollectionCSV = Umbraco.Content(dynamicPublishedMNTPNodeListCSV);
	    foreach (var item in dynamicMNTPCollectionCSV)
	    {     
	        <p>@item.Name</p>         
	    }       
	}


##Razor Macro (DynamicXml) Example

For XML data storage (This example returns a DynamicNodeList so that filtering and ordering can be used):

	@using umbraco.MacroEngines
	@using umbraco;
	@inherits umbraco.MacroEngines.DynamicNodeContext	 
	@{
	    if (Model.HasValue("mntpFeaturePickerXML"))
	    {
	        int[] nodeList = uQuery.GetXmlIds(Model.mntpFeaturePickerXML.ToXml());      
	        IEnumerable<DynamicNode> PublishedNodeList = Library.NodesById(nodeList.ToList());        
	        PublishedNodeList = PublishedNodeList.Where(x => x.GetType() != typeof(DynamicNull) && x.Id > 0);                        
	        dynamic multiNodeTreePicker = new DynamicNodeList(PublishedNodeList);
	
	        if (multiNodeTreePicker.Any())
	        {
	            foreach (var item in multiNodeTreePicker.Where("Visible"))
	            {                   
	                <p>@item.Name</p>      
	            }               
	        }
	    } 
	}

For CSV data storage (This example returns a DynamicNodeList so that filtering and ordering can be used):

	@using umbraco.MacroEngines
	@inherits umbraco.MacroEngines.DynamicNodeContext
	@{
	    if (Model.HasValue("mntpFeaturePickerCSV"))
	    {
	        IEnumerable<DynamicNode> PublishedNodeList = Library.NodesById(Model.mntpFeaturePickerCSV.Split(','));        
	        PublishedNodeList = PublishedNodeList.Where(x => x.GetType() != typeof(DynamicNull) && x.Id > 0);                        
	        dynamic multiNodeTreePicker = new DynamicNodeList(PublishedNodeList);
	
	        if (multiNodeTreePicker.Any())
	        {
	            foreach (var item in multiNodeTreePicker.Where("Visible"))
	            {                   
	                <p>@item.Name</p>      
	            }               
	        }
	    } 
	}

##XSLT Macro Example

For XML data storage:

	<xsl:if test="$currentPage/mntpFeaturePicker/MultiNodePicker/nodeId">
		<xsl:for-each select="$currentPage/mntpFeaturePicker/MultiNodePicker/nodeId">
			<xsl:variable name="currentnode" select="umbraco.library:GetXmlNodeById(.)" />
			<!-- render only published nodes -->
			<xsl:if test="count($currentnode/error) = 0">			
				<p><xsl:value-of select="$currentnode/@nodeName" /></p>
			</xsl:if>			
		</xsl:for-each>
	</xsl:if>

For CSV data storage:

	<xsl:if test="string-length($currentPage/mntpFeaturePicker) > 0">  
	  <xsl:variable name="items" select="umbraco.library:Split($currentPage/mntpFeaturePicker,',')" />  
	  <xsl:for-each select="$items//value">
	  	<xsl:variable name="currentnode" select="umbraco.library:GetXmlNodeById(.)" />
		<!-- render only published nodes -->
		<xsl:if test="count($currentnode/error) = 0">			
			<p><xsl:value-of select="$currentnode/@nodeName" /></p>
		</xsl:if>		  		  		
	  </xsl:for-each>	  
	</xsl:if>
