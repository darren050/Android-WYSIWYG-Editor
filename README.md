Laser-Native-Editor
===================

*Laser-Native-Editor** is a WYSIWYG editor completely written in Android using the native components in the editor tree.


Demo
-------------

![enter image description here](/screens/gif-1.gif)&nbsp;&nbsp;&nbsp;&nbsp;![enter image description here](/screens/gif-2.gif)&nbsp;&nbsp;&nbsp;&nbsp;![enter image description here](/screens/gif-3.gif)&nbsp;&nbsp;&nbsp;&nbsp;![enter image description here](https://scontent-kul1-1.xx.fbcdn.net/v/t1.0-9/13227159_269389593411500_6219375228375154304_n.jpg?oh=f6b5fe0ab000d0eb89915d0147ff24ef&oe=579B3038)&nbsp;&nbsp;&nbsp; ![enter image description here](https://scontent-kul1-1.xx.fbcdn.net/v/t1.0-9/13177651_269390020078124_2120913121972781573_n.jpg?oh=4a94f50c352330007063dc7dd092da0d&oe=57E6CE25)  


Features
-------------

 - **Renderer or Editor**: You can use Laser as a **Renderer** to Render the content in your views. Or use it as an **Editor** to create the content.
 - **No Webviews used** to render the content. Laser uses Native EditText, ImageView and as such to render the contents.
 - **HTML Parser:** Render your HTML Code into the editor and vice versa.
 - **Image Uploader Api:** Use the Built-in API to upload the images to the server.
 - Flexibility to create your own Toolbar layout. Just hookup the API methods on the click events.
 - Image Orientation: Fit width, Center Crop or Stretch.

The editor is built, so that every part of the design have been exposed and is available for customization. You can define, how the editor should look like, and what are the controls, that should be available (the controls toolbar layout can also be created by yourself, just call the API methods on the click event).

**Available Controls:**

| Control     | Usage |
| :------- | :-----: |
| `H1`,  `H2` and `H3` | Insert Headings | 
| `Bold`, `Italic`, `Intent` & `Outdent`    | Format the text   | 
| `Image Picker`| Insert Images to the editor from storage or a URL    | 
| `Hyperlinks` | Add Links to the editor
|`Location Selector` | Use the embedded map editor to tag and insert locations to the editor |
|`Numbered` and `Bulleted` Lists | Let's you created Unorderd and Ordered lists |
|`Line Divider` | Add a divider among paragraphs or Headings
|`Clear Content` | Remove all contents from the editor



Usage
-------------------

**Layout XML**

  

    <com.irshu.libs.Editor
        android:layout_width="match_parent"
        android:id="@+id/editor"
        app:render_type="Editor"
        app:placeholder="Start writing here..."
        android:paddingTop="10dp"
        android:paddingLeft="10dp"
        android:paddingRight="10dp"
        android:layout_height="wrap_content"
        android:paddingBottom="100dp"
    />

**Activity**


	 

 
     @Override
     protected void onCreate(Bundle savedInstanceState) {

        _editor= (Editor) findViewById(R.id.editor);
        
        findViewById(R.id.action_header_1).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                _editor.UpdateTextStyle(ControlStyles.H1);
            }
        });
        findViewById(R.id.action_bold).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                _editor.UpdateTextStyle(ControlStyles.BOLD);
            }
        });

        findViewById(R.id.action_Italic).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                _editor.UpdateTextStyle(ControlStyles.ITALIC);
            }
        });
        findViewById(R.id.action_bulleted).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                _editor.InsertList(false);
            }
        });
        findViewById(R.id.action_unordered_numbered).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                _editor.InsertList(true);
            }
        });
        findViewById(R.id.action_hr).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                _editor.InsertDivider();
            }
        });
        findViewById(R.id.action_insert_image).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                _editor.OpenImagePicker();
            }
        });
        findViewById(R.id.action_insert_link).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                _editor.InsertLink();
            }
        });
        findViewById(R.id.action_map).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                _editor.InsertMap();
            }
        });
    }
        
    
        

If you are using **Image Pickers** or **Map Marker Pickers**, Add the following into your **Activity**:

    
     @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {

        if (requestCode == _editor.PICK_IMAGE_REQUEST && resultCode == Activity.RESULT_OK&& data != null && data.getData() != null) {
            try {
                Bitmap bitmap = MediaStore.Images.Media.getBitmap(getContentResolver(), uri);
                _editor.InsertImage(bitmap);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        else if (resultCode == Activity.RESULT_CANCELED) {
            //Write your code if there's no result
            _editor.RestoreState();
        }
        else if(requestCode== _editor.MAP_MARKER_REQUEST){
            _editor.InsertMap(data.getStringExtra("cords"), true);
        }
    }


###Adding Callbacks

     _editor.setEditorListener(new BaseClass.EditorListener() {
                @Override
                public void onTextChanged(EditText editText, Editable text) {
                   // Toast.makeText(EditorTestActivity.this, text,Toast.LENGTH_SHORT).show();
                }
            });

##API

 - `UpdateTextStyle(EditorTextStyle style);` Update the text style for
   the currently active block. Possible values are `H1,H2,H3,BOLD,ITALIC,INDENT and OUTDENT`
   
 - `InsertList(boolean isOrdered);`Insert an Ordered or Unordered List. 
 - `InsertDivider();` Insert a line divider
 
 - `InsertLink():` Insert a link to the editor. Calling this method will open the dialog, where the user can type in the URL.
 
 - `OpenImagePicker();` Opens up the image picker. Once the user has selected the image, it's automatically inserted to the editor. But you must configure a remote URL ,where you want the image to be uploaded. If the Remote URL is not specifed, the image is not persisted.

 - `InsertImage(Bitmap bitmap);` Insert a bitmap image into the editor.

 - `setImageUploaderUri(String Url);`used to configure the remote URL ,where you want the image to be uploaded. This is compulsory if you are using the Image Picker.

 - `InsertMap():` Fires up the google map location picker activity. Once the user has selected the location, the library will automatically insert the marker with the location into editor.

 - `InsertMap(data.getStringExtra("cords"), true);` Insert the marker into the editor. This method is intented to use inside `onActivityResult():`
 
 - `RenderEditor(String html);` Used to render the content into the editor from an HTML string
 
 - `getContent();`  returns the content in the editor as `EditorState`
 
 - `getContentAsSerialized();` returns the content as serialized form of EditorState
 
 - `getContentAsSerialized(EditorState state);` returns the provided parameter as serialized.
 - `getContentAsHTML();` returns the editor content in HTML format. 


##Future Improvements


 - Insert quotes.
 - Underline and Overline selections.
 - Imrove and add more callbacks.

Contributions are much appreciated, feel free to fork and customize for your needs.
