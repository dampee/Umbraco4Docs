##BackOffice
These items are common terms and concepts that are used throughout the Umbraco backoffice.

### [Document Type](Document-Types/index.md)
Document types define the types of pages/nodes that backoffice users can create in the content tree. Each document type contains different properties or fields.
Each field has a specific data type e.g. text, number, ... 

###Content
Content are the pages and content.  Each item in the tree is called a Node.  Each node in the content tree exists out of different fields.  Every content item (or Node) is defined by a Document Type.   

###[Data Type](Data-Types/index.md)
Each document type property has a data type which defines the type of input of that property. Data types reference a Property Editor and are configured in the Umbraco backoffice in the developer section.  A datatype can be something very simple (textstring, number, true/false,...) or more complex (multi node tree picker, image cropper, ...)

###Media Type
Media types are very similar to document types in Umbraco except they are specifically for media items in the media section.

###Media
Media items are used to store assets like images and video within the Media section and can be referenced from your content.

###Member Type
Similar to a Document type and a Media type. You are able to define custom properties to store on a member such as twitter username or website URL for example.

###[Member](Members/index.md)
A member is someone who has access to signup, register and login into your **public website** and is not to be confused with User.

###Node
A node is an item in the Content tree in the Umbraco backoffice content section

###User
A user is someone who has access to the **Umbraco backoffice** and is not to be confused with Member.

### [Macros](../../Reference/Templating/Macros/index.md)

A macro is a reusable piece of functionality that you can re-use throughout your site. Macros can be configured with parameters and be inserted into a Rich Text Editor. Your can define what macros are available for your editors to insert in to the rich text editor. When an editor inserts a macro into the rich text editor it will prompt them to fill out any of the defined parameters for the macro.

### [Templates / Layouts / Masterpages](../../Reference/Templating/index.md)
A template is where you define the HTML markup of your website. A layout is a common template that contains common markup such as the `<head>` section.

###Template Sections
Template sections allow child templates that inherit the master layout template to insert html code up into the main layout template. For example a child template inserting code into the `<head>` tag

### [Applications/Sections](../../Extending-Umbraco/Section-Trees/index.md)
An application in Umbraco is where you do specific tasks related to that application. For example content, settings, developer. But it is possible to develop your own custom applications to work with your own data.

###Properties
Every document type has properties. These are the fields that the content editor is allowed to edit for the node.

### [Property Editors](Property-Editors/index.md)
A property editor is a way to insert content into Umbraco. An example of a property editor is the Rich Text Editor. It may be confused with Data Types. Its possible to have many Rich Text Editor Data Types with different settings that all use the Rich Text Editor property editor.

### [Macro Parameter Editor](../../Extending-Umbraco/Macro-Parameter-Editors/index.md)
A parameter editor defines the usage of a property editor for use as a parameter for Macros.

###Editor
An editor is what you use to edit different items within the backoffice. There are editors specific to editing stylesheets, there are editors for editing Macros and so forth.

###[Tree](../../Extending-Umbraco/Section-Trees/index.md)
A tree is an hierarchical list of items related (and usually restricted) to a specific concept, which could be something like a content tree or a media tree.

### [Dashboards](Extending-Umbraco/Dashboards/index.md)
A dashboard is the main view you are presented with when entering a section within the backoffice, and can be used to show valuable information to the users of the system.

###Package
A package is the Umbraco term for a module or plug-in used to extend Umbraco