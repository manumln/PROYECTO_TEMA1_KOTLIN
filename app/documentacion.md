# PROYECTO TEMA 1 KOTLIN

Este proyecto es una aplicación de Android que proporciona varias funcionalidades a los usuarios a través de una interfaz gráfica. La aplicación consta de cuatro botones, cada uno de los cuales realiza una acción específica:

## Llamada Telefónica

Al hacer clic en este botón, la aplicación inicia una llamada telefónica.

La acción está dirigida a otra actividad llamada CallActivity.

```java
botonLlamada.setOnClickListener {
            val intent = Intent(this, CallActivity::class.java)
            startActivity(intent)
        }
```
### CallActivity


El código verifica si la aplicación tiene permiso para realizar llamadas (CALL_PHONE).

Si la aplicación tiene el permiso, se hace la llamada de emergencia.

Si no tiene el permiso, se le solicita al usuario el  permiso para hacer llamadas.

Función makePhoneCall:
Esta función realiza la llamada de emergencia al número 911 utilizando un Intent con la acción ACTION_CALL.

Botón de Llamada de Emergencia:
Cuando se hace clic en el botón, se ve si la aplicación tiene el permiso.

```java
class CallActivity : AppCompatActivity() {

    private val CALL_PERMISSION_REQUEST_CODE = 112

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_call)

        val emergencyCallButton = findViewById<Button>(R.id.BotonEmergencias)

        emergencyCallButton.setOnClickListener {
            if (ContextCompat.checkSelfPermission(
                    this,
                    Manifest.permission.CALL_PHONE
                ) == PackageManager.PERMISSION_GRANTED
            ) {
                makePhoneCall()
            } else {
                // Solicitar el permiso al usuario
                requestPermissionLauncher.launch(Manifest.permission.CALL_PHONE)
            }
        }
    }

    private val requestPermissionLauncher =
        registerForActivityResult(ActivityResultContracts.RequestPermission()) { isGranted ->
            if (isGranted) {
                makePhoneCall()
            } else {
            }
        }

    private fun makePhoneCall() {
        val emergencyNumber = "tel:911"

        // permiso? CALL_PHONE
        if (ContextCompat.checkSelfPermission(
                this,
                Manifest.permission.CALL_PHONE
            ) == PackageManager.PERMISSION_GRANTED
        ) {
            val callIntent = Intent(Intent.ACTION_CALL)
            callIntent.data = Uri.parse(emergencyNumber)
            startActivity(callIntent)
        }
    }
}
```
## Alarma en 2 minutos

El codigo hace llamada al metodo de crearAlarma.

```java 
botonAlarma.setOnClickListener {
            Toast.makeText(this@MainActivity, "Pulsado boton Alarma", Toast.LENGTH_SHORT).show()
            crearAlarma()
        }
```
El código crea una alarma para sonar en 2 minutos desde el momento en que se ejecuta.

```java
private fun crearAlarma() {
        val alarma = Calendar.getInstance()
        alarma.add(Calendar.MINUTE, 2) // Add 2 minutes to the current time

        val intent = Intent(AlarmClock.ACTION_SET_ALARM)
        intent.putExtra(AlarmClock.EXTRA_MESSAGE, "Alarma en 2 minutos")
        intent.putExtra(AlarmClock.EXTRA_HOUR, alarma.get(Calendar.HOUR_OF_DAY))
        intent.putExtra(AlarmClock.EXTRA_MINUTES, alarma.get(Calendar.MINUTE))

        if (intent.resolveActivity(packageManager) != null) {
            startActivity(intent)
        } else {
            Toast.makeText(this, "No se puede crear la alarma", Toast.LENGTH_SHORT).show()
        }
    }
```

## Navegador hacia una URL
El codigo hace llamada al metodo de crearAlarma y ademas escribimos la direccion que queremos que nos dirija.

```java
Log.d("MainActivity", "Clic en el botón de URL")
            val url = "https://www.google.es"
            openWebPage(url)
        }
```

Cuando se clic en el botón botonURL, se registra el evento, se almacena una URL en la variable url y se abre esa URL en el navegador web utilizando la función openWebPage. En este caso se abre google.es.

```java
private fun openWebPage(url: String) {
        val webpage = Uri.parse(url)
        val intent = Intent(Intent.ACTION_VIEW, webpage)
        if (intent.resolveActivity(packageManager) != null) {
            startActivity(intent)
        }
    }
```

## Abrir un álbum en Spotify

El codigo hace llamada al metodo de abrir Spotify y ademas escribimos la direccion del album/canción que queremos que nos dirija.

```java
botonMusica.setOnClickListener {
            abrirSpotify("spotify:track:6nK2pIKFcRc5frrZKHgsiT");
        }
```

Esta función permite abrir un enlace de Spotify (que anteriormente le hemos escrito) en la aplicación de Spotify si esta está instalada en el dispositivo. Si la aplicación de Spotify no está instalada, se muestra un mensaje.

```java
private fun abrirSpotify(url: String) {
        val spotifyDeepLink = Uri.parse(url)

        //
        val uri = Uri.parse(spotifyDeepLink.toString())
        val intent = Intent(Intent.ACTION_VIEW, uri)
        if (intent.resolveActivity(packageManager) != null) {
            startActivity(intent)
        } else {
            Log.d("MainActivity", "Spotify no está instalado en el dispositivo.")
        }
    }
```
##XML 
###activity_main

Esta activity incluye botones con imágenes y un icono en la parte superior de la pantalla. Cada botón está destinado a realizar una acción específica; abrir una URL, realizar una llamada telefónica, programar una alarma y abrir un enlace de Spotify.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/white"
    tools:context=".MainActivity">

    <ImageButton
        android:id="@+id/botonMusica"
        android:layout_width="84dp"
        android:layout_height="81dp"
        android:layout_margin="16dp"
        android:layout_marginBottom="172dp"
        android:background="#FFFFFF"
        android:scaleType="fitCenter"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintStart_toEndOf="@+id/botonURL"
        app:layout_constraintTop_toBottomOf="@+id/botonAlarma"
        app:layout_constraintVertical_bias="0.5"
        app:srcCompat="@drawable/auriculares" />

    <ImageButton
        android:id="@+id/botonURL"
        android:layout_width="84dp"
        android:layout_height="81dp"
        android:layout_margin="16dp"
        android:background="#FFFFFF"
        android:scaleType="fitCenter"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toStartOf="@+id/botonMusica"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/botonLlamada"
        app:layout_constraintVertical_bias="0.5"
        app:srcCompat="@drawable/navegador" />

    <ImageButton
        android:id="@+id/botonLlamada"
        android:layout_width="84dp"
        android:layout_height="81dp"
        android:layout_margin="16dp"
        android:layout_marginBottom="459dp"
        android:background="#FFFFFF"
        android:scaleType="fitCenter"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toStartOf="@+id/botonAlarma"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.5"
        app:srcCompat="@drawable/telefono" />

    <ImageButton
        android:id="@+id/botonAlarma"
        android:layout_width="84dp"
        android:layout_height="81dp"
        android:layout_margin="16dp"
        android:layout_marginBottom="459dp"
        android:background="#FFFFFF"
        android:scaleType="fitCenter"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintStart_toEndOf="@+id/botonLlamada"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.5"
        app:srcCompat="@drawable/alarma" />

    <ImageView
        android:id="@+id/icon"
        android:layout_width="189dp"
        android:layout_height="196dp"
        android:layout_marginStart="111dp"
        android:layout_marginEnd="111dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:srcCompat="@drawable/dom"
        tools:layout_editor_absoluteY="30dp" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

###activity_call

Esta activity con una imagen de un teléfono de emergencia en el centro y un botón en la parte inferior que permite a los usuarios llamar a emergencias al hacer clic en él. 

```XML
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/relativeLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp"
    tools:context=".CallActivity">

    <ImageView
        android:id="@+id/imageView3"
        android:layout_width="198dp"
        android:layout_height="187dp"
        android:layout_centerHorizontal="true"
        android:layout_centerVertical="true"
        android:contentDescription="Imagen de teléfono de emergencia"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@drawable/telefono" />

    <Button
        android:id="@+id/BotonEmergencias"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="556dp"
        android:background="#FF0000"
        android:backgroundTint="#ABA2FE"
        android:backgroundTintMode="add"
        android:text="Llamar a Emergencias"
        android:textColor="#FFFFFF"
        app:icon="@drawable/amanecer"
        app:layout_constraintHorizontal_bias="0.497"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

##IMAGENES DEL PROYECTO

![Esta es una imagen de ejemplo](https://i.ibb.co/VgdqXCP/menu.png)
![Esta es una imagen de ejemplo](https://i.ibb.co/xfWKqv0/Screenshot-20231029-002531.png)
![Esta es una imagen de ejemplo](https://i.ibb.co/RH2WDh3/permisos.png)
![Esta es una imagen de ejemplo](https://i.ibb.co/fdsd5BT/folklore.png)
![Esta es una imagen de ejemplo](https://i.ibb.co/h930KZN/navegador.png)



