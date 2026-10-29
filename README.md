# Practical: Develop an Android Application Using GridView with Custom Adapter

## Aim

To develop an Android application using **GridView and a Custom Adapter** to display images with their corresponding names in a two-column grid layout. The application also displays a Toast message when an item is selected.

## Project Details

- **Project Name:** `GridViewDemo`
- **Platform:** Android
- **Programming Language:** Java
- **Layout:** XML
- **Adapter:** Custom Adapter using `BaseAdapter`

## Technologies Used

- Android Studio
- Java
- XML
- Android GridView
- Custom Adapter (`BaseAdapter`)
- ImageView
- TextView
- Toast

## Project Structure

```text
GridViewDemo/
├── app/
│   └── src/
│       └── main/
│           ├── java/
│           │   └── com/
│           │       └── example/
│           │           └── gridviewdemo/
│           │               ├── MainActivity.java
│           │               └── CustomAdapter.java
│           ├── res/
│           │   └── layout/
│           │       ├── activity_main.xml
│           │       └── grid_item.xml
│           └── AndroidManifest.xml
└── README.md
```

## 1. Create the Main Layout

**Filename:** `activity_main.xml`

**Location:** `app/src/main/res/layout/activity_main.xml`

This layout contains a heading and a GridView with two columns.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="10dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:paddingVertical="20dp"
        android:paddingBottom="10dp"
        android:text="Image Gallery"
        android:textSize="24sp"
        android:textStyle="bold" />

    <GridView
        android:id="@+id/gridView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:numColumns="2"
        android:verticalSpacing="10dp"
        android:horizontalSpacing="10dp"
        android:stretchMode="columnWidth" />

</LinearLayout>
```

## 2. Create the Grid Item Layout

**Filename:** `grid_item.xml`

**Location:** `app/src/main/res/layout/grid_item.xml`

Create a new Layout XML File named `grid_item`. This layout defines the appearance of each item in the grid, including an image and its name.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:gravity="center"
    android:padding="10dp">

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="120dp"
        android:layout_height="120dp"
        android:scaleType="centerCrop" />

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="18sp"
        android:paddingTop="5dp" />

</LinearLayout>
```

## 3. Create the Custom Adapter Class

**Filename:** `CustomAdapter.java`

**Location:** `app/src/main/java/com/example/gridviewdemo/CustomAdapter.java`

The Custom Adapter extends `BaseAdapter` and connects the image names and image resources to the individual grid items.

```java
package com.example.gridviewdemo;

import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.BaseAdapter;
import android.widget.ImageView;
import android.widget.TextView;

public class CustomAdapter extends BaseAdapter {

    Context context;
    String[] imageNames;
    int[] images;
    LayoutInflater inflater;

    public CustomAdapter(Context context, String[] imageNames, int[] images) {
        this.context = context;
        this.imageNames = imageNames;
        this.images = images;
        inflater = LayoutInflater.from(context);
    }

    @Override
    public int getCount() {
        return imageNames.length;
    }

    @Override
    public Object getItem(int position) {
        return imageNames[position];
    }

    @Override
    public long getItemId(int position) {
        return position;
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {

        View view = inflater.inflate(R.layout.grid_item, parent, false);

        ImageView imageView = view.findViewById(R.id.imageView);
        TextView textView = view.findViewById(R.id.textView);

        imageView.setImageResource(images[position]);
        textView.setText(imageNames[position]);

        return view;
    }
}
```

## 4. Write the Main Activity

**Filename:** `MainActivity.java`

**Location:** `app/src/main/java/com/example/gridviewdemo/MainActivity.java`

The Main Activity initializes the GridView, supplies the image names and image resources, attaches the Custom Adapter, and handles item clicks.

```java
package com.example.gridviewdemo;

import android.os.Bundle;
import android.widget.GridView;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private final String[] imageNames = {
        "Nature",
        "Flower",
        "Mountain",
        "Beach"
    };

    private final int[] images = {
        android.R.drawable.ic_menu_gallery,
        android.R.drawable.ic_menu_camera,
        android.R.drawable.ic_menu_compass,
        android.R.drawable.ic_menu_mapmode
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        GridView gridView = findViewById(R.id.gridView);

        gridView.setAdapter(
            new CustomAdapter(this, imageNames, images)
        );

        gridView.setOnItemClickListener(
            (parent, view, position, id) ->
                Toast.makeText(
                    this,
                    "Selected: " + imageNames[position],
                    Toast.LENGTH_SHORT
                ).show()
        );
    }
}
```

## 5. Android Manifest

**Filename:** `AndroidManifest.xml`

The launcher activity must be declared in the manifest. Android Studio usually generates this file automatically. Check that your activity declaration includes the launcher intent filter.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <application
        android:allowBackup="true"
        android:label="GridViewDemo"
        android:supportsRtl="true"
        android:theme="@style/Theme.GridViewDemo">

        <activity
            android:name=".MainActivity"
            android:exported="true">

            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>

        </activity>

    </application>

</manifest>
```

**Note:** Keep the theme name generated by your own Android Studio project if it differs from `@style/Theme.GridViewDemo`. Merge the activity declaration into your existing manifest if it already contains other required settings.

## How to Run the Application

1. Open Android Studio.
2. Create a new Android project named `GridViewDemo`.
3. Select Java as the programming language.
4. Create `activity_main.xml` in the `res/layout` folder.
5. Create `grid_item.xml` in the `res/layout` folder.
6. Create the Java class `CustomAdapter.java` in the package `com.example.gridviewdemo`.
7. Add the provided code to `MainActivity.java`.
8. Ensure the package name matches your project's application package.
9. Sync the project and resolve any build errors.
10. Run the application on an Android emulator or a connected Android device.

## Expected Output

When the application runs, it displays an **Image Gallery** heading followed by a grid containing four items arranged in two columns.

| Item | Display Name |
|---|---|
| 1 | Nature |
| 2 | Flower |
| 3 | Mountain |
| 4 | Beach |

The images used in this practical are built-in Android drawable icons, so additional image files are not required.

When the user taps an item, a Toast message appears. For example:

```text
Selected: Nature
```

**Note:** The built-in drawable icons are illustrative icons rather than actual nature, flower, mountain, and beach photographs.

## Working of the Application

1. **MainActivity:** Loads the main XML layout and finds the GridView.
2. **Data Arrays:** Stores the names and corresponding drawable resource IDs.
3. **CustomAdapter:** Extends `BaseAdapter` and supplies data for each grid item.
4. **LayoutInflater:** Inflates `grid_item.xml` to create each item's view.
5. **ImageView:** Displays the drawable resource corresponding to the item.
6. **TextView:** Displays the name of the item.
7. **GridView:** Arranges the generated item views into two columns.
8. **Item Click Listener:** Detects the selected position and displays its name using a Toast message.

## Important Methods

| Method | Purpose |
|---|---|
| `getCount()` | Returns the total number of items. |
| `getItem()` | Returns the item at the specified position. |
| `getItemId()` | Returns the ID associated with the position. |
| `getView()` | Creates and returns the view for each grid item. |
| `setAdapter()` | Connects the Custom Adapter to the GridView. |
| `setOnItemClickListener()` | Handles clicks on grid items. |
| `setImageResource()` | Sets the image displayed by the ImageView. |
| `Toast.makeText()` | Displays a short message to the user. |

## Advantages of GridView with Custom Adapter

- Displays items in a structured grid format.
- Supports custom layouts containing images and text.
- Separates data handling from the user interface.
- Handles item selection through click listeners.
- Can be adapted for image galleries, product catalogs, and category menus.

## Result

The Android application **GridViewDemo** was developed successfully using GridView and a Custom Adapter. It displays four items in a two-column grid with images and names. When an item is selected, the application displays a Toast message containing the selected item's name.

## Conclusion

This practical demonstrates how to implement a GridView using a Custom Adapter in Android. It explains how to define a custom item layout, bind data to ImageView and TextView components, and handle user interaction through an item click listener.

---

**Practical Completed Successfully.**
