
# Ex.No:3 Create Your Own Content Providers to get Contacts details.


## AIM:

To create your own content providers to get contacts details using Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min. required Artic Fox)

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as “ex.no.3″ and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Get contacts details and Display details give in MainActivity file.

Step 7: Save and run the application.

## PROGRAM:
```
/*
Program to print the text create your own content providers to get contacts details.
Developed by: SAI DESHIYA K
Registeration Number : 212224220084
*/
```
## MainActivity.java:
```
package com.example.contact;

import android.Manifest;
import android.content.pm.PackageManager;
import android.database.Cursor;
import android.net.Uri;
import android.os.Bundle;
import android.provider.ContactsContract;
import android.text.method.ScrollingMovementMethod;
import android.util.Log;
import android.widget.Button;
import android.widget.TextView;

import androidx.activity.EdgeToEdge;
import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {

    Button button;
    TextView contactResult;

    private static final int CONTACT_PERMISSION_CODE = 100;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        button = findViewById(R.id.button);
        contactResult = findViewById(R.id.contactResult);
        contactResult.setMovementMethod(new ScrollingMovementMethod());

        button.setOnClickListener(v -> {
            Log.i("CONTACT_PROVIDER_DEMO", "BUTTON CLICKED");
            getPhoneContacts();
        });
    }

    private void getPhoneContacts() {

        if (ContextCompat.checkSelfPermission(
                this, Manifest.permission.READ_CONTACTS)
                != PackageManager.PERMISSION_GRANTED) {

            ActivityCompat.requestPermissions(
                    this,
                    new String[]{Manifest.permission.READ_CONTACTS},
                    CONTACT_PERMISSION_CODE
            );

            return;
        }

        displayContacts();
    }

    private void displayContacts() {

        StringBuilder contacts = new StringBuilder();

        Uri uri = ContactsContract.CommonDataKinds.Phone.CONTENT_URI;

        String[] projection = {
                ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME,
                ContactsContract.CommonDataKinds.Phone.NUMBER
        };

        try (Cursor cursor = getContentResolver().query(
                uri, projection, null, null,
                ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME + " ASC")) {

            if (cursor != null && cursor.moveToFirst()) {

                int count = cursor.getCount();

                Log.i("CONTACT_PROVIDER_DEMO",
                        "TOTAL CONTACTS: " + count);

                contacts.append("TOTAL CONTACTS: ")
                        .append(count).append("\n\n");

                do {

                    String name = cursor.getString(0);
                    String number = cursor.getString(1);

                    contacts.append("Name: ")
                            .append(name)
                            .append("\nNumber: ")
                            .append(number)
                            .append("\n\n");

                    Log.i("CONTACT_PROVIDER_DEMO",
                            "Name: " + name + " Number: " + number);

                } while (cursor.moveToNext());

                contactResult.setText(contacts.toString());

            } else {

                contactResult.setText(getString(R.string.no_contacts_found));
                Log.i("CONTACT_PROVIDER_DEMO", "No contacts found.");
            }

        } catch (Exception e) {

            contactResult.setText(getString(R.string.no_contacts_found));

            Log.e("CONTACT_PROVIDER_DEMO",
                    "Error retrieving contacts", e);
        }
    }

    @Override
    @SuppressWarnings("IfCanBeSwitch")
    public void onRequestPermissionsResult(
            int requestCode,
            @NonNull String[] permissions,
            @NonNull int[] grantResults) {

        super.onRequestPermissionsResult(
                requestCode, permissions, grantResults);

        if (requestCode == CONTACT_PERMISSION_CODE) {
            if (grantResults.length > 0
                    && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                displayContacts();
            } else {
                contactResult.setText(getString(R.string.permission_denied));
            }
        }
    }
}
```
## activity_main.xml:
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#F1F5F9">

    <TextView
        android:id="@+id/title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/title_content_provider"
        android:textColor="#172554"
        android:textSize="26sp"
        android:textStyle="bold"
        android:fontFamily="sans-serif"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toTopOf="@id/subtitle"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="65dp"
        android:layout_marginBottom="12dp" />

    <TextView
        android:id="@+id/subtitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/subtitle_access_contacts"
        android:textColor="#64748B"
        android:textSize="16sp"
        app:layout_constraintTop_toBottomOf="@id/title"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <Button
        android:id="@+id/button"
        android:layout_width="220dp"
        android:layout_height="58dp"
        android:text="@string/get_contacts"
        android:textSize="18sp"
        android:textStyle="bold"
        android:textColor="#FFFFFF"
        android:backgroundTint="#2563EB"
        app:layout_constraintTop_toBottomOf="@id/subtitle"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="35dp" />

    <TextView
        android:id="@+id/contactResult"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:layout_marginStart="20dp"
        android:layout_marginEnd="20dp"
        android:layout_marginTop="25dp"
        android:layout_marginBottom="20dp"
        android:background="#FFFFFF"
        android:padding="16dp"
        android:text="@string/contacts_placeholder"
        android:textColor="#1E293B"
        android:textSize="16sp"
        android:gravity="top"
        android:scrollbars="vertical"
        app:layout_constraintTop_toBottomOf="@id/button"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```
## AndroidManifest.xml:
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <uses-permission android:name="android.permission.READ_CONTACTS" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Contact">

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

## OUTPUT:
<img width="1920" height="1200" alt="Screenshot (271)" src="https://github.com/user-attachments/assets/86a7f003-22db-441b-8295-587d708abc4e" />
<img width="1920" height="1200" alt="Screenshot (272)" src="https://github.com/user-attachments/assets/b1c0dc54-ecbe-4a41-9d24-2363260a195b" />





## RESULT
Thus a Simple Android Application create your own content providers to get contacts details using Android Studio is developed and executed successfully.
