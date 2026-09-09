# Ex.No:6 Create a simple application to request storage and camera permission at RunTime using android studio.


## AIM:

To develop a simple application for RunTime Permission in Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min.required Giraffe)

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as runtimepermission and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Display process of runtimepermission in android mobile devices.

Step 7: Save and run the application.

## PROGRAM:
```
/*
Program to print the process of runtimepermission in android mobile devices”.
Developed by: Manisha selvakumari.S.S.
Registeration Number : 212223220055
*/
```
## Androidmanifest.xml:
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.RuntimePermissionDemo">
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:windowSoftInputMode="adjustResize">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```
## MainActivity.java:
```
package com.example.runtimepermissiondemo;

import android.Manifest;
import android.content.pm.PackageManager;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;

public class MainActivity extends AppCompatActivity {

    private static final int PERMISSION_REQUEST_CODE = 100;

    private TextView tvStatus;
    private Button btnRequest;

    // List of permissions we need
    private String[] permissions = {
            Manifest.permission.CAMERA,
            Manifest.permission.READ_EXTERNAL_STORAGE,
            Manifest.permission.WRITE_EXTERNAL_STORAGE
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        tvStatus = findViewById(R.id.tvStatus);
        btnRequest = findViewById(R.id.btnRequest);

        btnRequest.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                checkAndRequestPermissions();
            }
        });

        // Show current status when app starts
        updateStatusText();
    }

    private void checkAndRequestPermissions() {

        boolean allGranted = true;

        for (String permission : permissions) {
            if (ContextCompat.checkSelfPermission(this, permission)
                    != PackageManager.PERMISSION_GRANTED) {

                allGranted = false;
                break;
            }
        }

        if (allGranted) {
            Toast.makeText(
                    this,
                    "All permissions already granted",
                    Toast.LENGTH_SHORT
            ).show();

        } else {
            // Request missing permissions
            ActivityCompat.requestPermissions(
                    this,
                    permissions,
                    PERMISSION_REQUEST_CODE
            );
        }
    }

    @Override
    public void onRequestPermissionsResult(
            int requestCode,
            @NonNull String[] permissions,
            @NonNull int[] grantResults) {

        super.onRequestPermissionsResult(
                requestCode,
                permissions,
                grantResults
        );

        if (requestCode == PERMISSION_REQUEST_CODE) {

            boolean cameraGranted = false;
            boolean storageGranted = false;

            for (int i = 0; i < permissions.length; i++) {

                if (permissions[i].equals(Manifest.permission.CAMERA)) {

                    cameraGranted =
                            (grantResults[i] == PackageManager.PERMISSION_GRANTED);

                } else if (
                        permissions[i].equals(Manifest.permission.READ_EXTERNAL_STORAGE)
                                || permissions[i].equals(
                                Manifest.permission.WRITE_EXTERNAL_STORAGE)) {

                    storageGranted =
                            (grantResults[i] == PackageManager.PERMISSION_GRANTED);
                }
            }

            if (cameraGranted && storageGranted) {

                Toast.makeText(
                        this,
                        "All permissions granted!",
                        Toast.LENGTH_LONG
                ).show();

            } else {

                Toast.makeText(
                        this,
                        "Some permissions denied. App may not work properly.",
                        Toast.LENGTH_LONG
                ).show();

                // Check if user selected "Never ask again"
                boolean showRationaleCamera =
                        ActivityCompat.shouldShowRequestPermissionRationale(
                                this,
                                Manifest.permission.CAMERA
                        );

                boolean showRationaleStorage =
                        ActivityCompat.shouldShowRequestPermissionRationale(
                                this,
                                Manifest.permission.READ_EXTERNAL_STORAGE
                        );

                if (!showRationaleCamera || !showRationaleStorage) {

                    Toast.makeText(
                            this,
                            "Permissions permanently denied. Go to Settings to enable.",
                            Toast.LENGTH_LONG
                    ).show();
                }
            }

            updateStatusText();
        }
    }

    private void updateStatusText() {

        boolean cameraGranted =
                ContextCompat.checkSelfPermission(
                        this,
                        Manifest.permission.CAMERA
                ) == PackageManager.PERMISSION_GRANTED;

        boolean readGranted =
                ContextCompat.checkSelfPermission(
                        this,
                        Manifest.permission.READ_EXTERNAL_STORAGE
                ) == PackageManager.PERMISSION_GRANTED;

        boolean writeGranted =
                ContextCompat.checkSelfPermission(
                        this,
                        Manifest.permission.WRITE_EXTERNAL_STORAGE
                ) == PackageManager.PERMISSION_GRANTED;

        String status =
                "Camera: " +
                        (cameraGranted ? "✅ GRANTED" : "❌ DENIED") +
                        "\n" +
                        "Read Storage: " +
                        (readGranted ? "✅ GRANTED" : "❌ DENIED") +
                        "\n" +
                        "Write Storage: " +
                        (writeGranted ? "✅ GRANTED" : "❌ DENIED");

        tvStatus.setText(status);
    }
}

```
## activity_main.xml:
```

<?xml version="1.0" encoding="utf-8"?>

<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvStatus"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Permissions Status: Not granted"
        android:textSize="18sp"
        android:textStyle="bold"
        android:layout_marginBottom="30dp" />

    <Button
        android:id="@+id/btnRequest"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Request Permissions"
        android:textSize="16sp" />

</LinearLayout>

```
## OUTPUT
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/981ca044-5f4f-4429-966a-73b2fe73c5f0" />




## RESULT
Thus a Simple Android Application to request storage and camera permission at RunTime in Android Studio is developed and executed successfully.
