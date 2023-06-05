<div justify="center">
  <img align="left" width="41" alt="logo_fivvy_collapse" src="https://github.com/IsaiasMella/readmeDeletear/assets/67384494/a5e66146-1c18-4e22-bd8e-201ef666aedb">
  <h1>Contextual-atirubutes-library</h1>
</div>




Este proyecto contiene varios módulos nativos en java que se encargan de recopilar información del usuario.

 


## :pencil: Documentation 
:point_right: GetContacts: obtiene la lista de contactos del dispositivo.

 

:point_right: GetAppUsage: obtiene la cantidad de tiempo que el usurio utiliza cada aplicación.

 

:point_right: GetDeviceInfo: obtiene la informacion del dispositivo móbil que utiliza la aplicación.

 

:point_right: GetInstalledApps:obtiene las aplicaciones instaladas del dispositivo.

 

:point_right: OpenUsageSettings: Abre la opción para habilitar los permisos sensibles(necesario para el de GetAppUsage)

 

 


## Run Locally

 

Clone the project

 

```bash
  git clone https://bitbucket.org/AIASAP/contextual-attributes-android-lib/src/master/
```

 

Go to the project directory

 

```bash
  cd contextual-attributes-library
```

 

Install dependencies

 

```bash
  npm install
```

 

Start the server

 

```bash
  npm run start
```

 


## AllUsage/Examples

 

```java
package com.example.contextual_attributes_library;

 

import android.Manifest;
import android.os.Bundle;
import android.view.View;
import android.widget.TextView;

 

import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;

 


import com.example.fivvycontextual.data.repositories.AppUsageRepositoryImpl;
import com.example.fivvycontextual.data.repositories.DeviceInfoRepositoryImpl;
import com.example.fivvycontextual.data.repositories.InstalledAppsRepositoryImpl;
import com.example.fivvycontextual.domain.usecases.GetAppUsage;
import com.example.fivvycontextual.domain.usecases.GetDeviceInfo;
import com.example.fivvycontextual.domain.usecases.GetInstalledApps;
import com.example.fivvycontextual.domain.usecases.OpenUsageSettings;
import com.example.fivvycontextual.domain.usecases.interfaces.IGetAppUsageUseCase;
import com.example.fivvycontextual.domain.usecases.interfaces.IGetDeviceInfoUseCase;
import com.example.fivvycontextual.domain.usecases.interfaces.IGetInstalledAppsUseCase;
import com.example.fivvycontextual.domain.usecases.interfaces.IOpenUsageSettingsUseCase;

 

import com.example.fivvycontextual.domain.usecases.interfaces.IGetContactsUseCase;
import com.example.fivvycontextual.data.repositories.ContactRepositoryImpl;
import com.example.fivvycontextual.domain.usecases.GetContacts;

 

import java.util.HashMap;
import java.util.Map;

 

public class MainActivity extends AppCompatActivity {
    private IGetContactsUseCase getContactsUseCase;
    private IGetAppUsageUseCase getAppUsageUseCase;
    private IOpenUsageSettingsUseCase openUsageAccessSettings;

 

    private IGetDeviceInfoUseCase getDeviceInfoUseCase;

 

    private IGetInstalledAppsUseCase getInstalledAppsUseCase;
    private final Map<Integer, Runnable> buttonCommands = new HashMap<>();

 

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.READ_CONTACTS}, 1);

 

        initializeViews();
        initializeButtonActions();
    }

 

    private void initializeViews() {
        findViewById(R.id.button).setOnClickListener(this::onClick);
        findViewById(R.id.button2).setOnClickListener(this::onClick);
        findViewById(R.id.button3).setOnClickListener(this::onClick);
        findViewById(R.id.button4).setOnClickListener(this::onClick);
        findViewById(R.id.button5).setOnClickListener(this::onClick);
    }

 

    private void initializeButtonActions() {
        openUsageAccessSettings = new OpenUsageSettings(new AppUsageRepositoryImpl(this));
        getContactsUseCase = new GetContacts(new ContactRepositoryImpl(getContentResolver()));
        getAppUsageUseCase = new GetAppUsage(new AppUsageRepositoryImpl(this));
        getDeviceInfoUseCase = new GetDeviceInfo(new DeviceInfoRepositoryImpl(this));
        getInstalledAppsUseCase = new GetInstalledApps(new InstalledAppsRepositoryImpl(this));

 

        buttonCommands.put(R.id.button, () -> updateTextView(R.id.textView, getInstalledAppsUseCase.execute().toString()));
        buttonCommands.put(R.id.button2, () -> {
            updateTextView(R.id.textView1, getContactsUseCase.execute().toString());
        });
        buttonCommands.put(R.id.button3, () -> updateTextView(R.id.textView2, getAppUsageUseCase.execute().toString()));
        buttonCommands.put(R.id.button4, () -> openUsageAccessSettings.execute());
        buttonCommands.put(R.id.button5, () -> updateTextView(R.id.textView3, getDeviceInfoUseCase.execute().toString()));
    }

 

    private void onClick(View view) {
        Runnable command = buttonCommands.get(view.getId());
        if (command != null) {
            command.run();
        } else {
            // Manejar el caso en que no haya comando para este botón
        }
    }

 

    private void updateTextView(int id, String text) {
        ((TextView) findViewById(id)).setText(text);
    }

 

}
```
