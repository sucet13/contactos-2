# contactos-2
package com.example.contactos;

import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;

import android.Manifest;
import android.app.Activity;
import android.content.Intent;
import android.content.pm.PackageManager;
import android.net.Uri;
import android.os.Bundle;
import android.view.View;
import android.widget.TextView;

public class Detalle extends AppCompatActivity {

    private TextView tvNombre;
    private TextView tvTelefono;
    private TextView tvemail;
    private TextView tvdescripcion;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_detalle);

        Bundle parametros = getIntent().getExtras();
        String nombre = parametros.getString(getResources().getString(R.string.pnombre));
        String telefono = parametros.getString(getResources().getString(R.string.ptelefono));
        String email = parametros.getString(getResources().getString(R.string.pemail));
        String descripcion = parametros.getString(getResources().getString(R.string.pdescripcion));

        tvNombre = (TextView) findViewById(R.id.tvnombre);
        tvTelefono = (TextView) findViewById(R.id.tvtelefono);
        tvemail = (TextView) findViewById(R.id.tvemail);
        tvdescripcion = (TextView) findViewById(R.id.tvdescripcion);

        tvNombre.setText(nombre);
        tvTelefono.setText(telefono);
        tvemail.setText(email);
        tvdescripcion.setText(descripcion);

    }

    public void llamar(View v) {
        String telefono = tvTelefono.getText().toString();


        if (ActivityCompat.checkSelfPermission (this, Manifest.permission.CALL_PHONE) != PackageManager.PERMISSION_GRANTED) {
            // TODO: Consider calling
            //    Activity#requestPermissions
            // here to request the missing permissions, and then overriding
            //   public void onRequestPermissionsResult(int requestCode, String[] permissions,
            //                                          int[] grantResults)
            // to handle the case where the user grants the permission. See the documentation
            // for Activity#requestPermissions for more details.
            return;
        }
        startActivity(new Intent(Intent.ACTION_CALL, Uri.parse("tel:" + telefono)));
    }
    public void enviaremail(View v){

        String name=tvemail.getText().toString() ;
        Intent emailIntent=new Intent((Intent.ACTION_SEND ));
        emailIntent .setData(Uri.parse("mailto:") );
        emailIntent .putExtra(Intent.EXTRA_EMAIL,emailIntent);
        emailIntent.setType("message/rfc822");
        startActivity(Intent.createChooser(emailIntent,"Email") ) ;
    }
}

Main Activity
package com.example.contactos;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ListView;

import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {

    ArrayList <contacto > contactos ;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        contactos= new ArrayList<contacto>();

        contactos.add(new contacto("Vania Chavez", "33511771", "chavez@gmail.com", "contadora" ) ) ;
        contactos.add(new contacto("Wendy Mansilla", "32548671", "45Wendy@gmail.com", "Secretaria" ) ) ;
        contactos.add(new contacto("Carolay Yol", "98752127", "carolay12@gmail.com", "Administradora" ) ) ;
        contactos.add(new contacto("Dania Lopez", "86528792", "d0lop@gmail.com", "amiga" ) ) ;
        contactos.add(new contacto("Alfredo Lorenzo", "97823465", "SAntos3@gmail.com", "Fatografo" ) ) ;
        contactos.add(new contacto("Luis Lopez", "84526472", "LopezLuis6@gmail.com", "Contador" ) ) ;


        ArrayList <String>  nombresContacto=new ArrayList<>() ;
        for (contacto contacto: contactos){
            nombresContacto.add(contacto.getNombre());


        }

        ListView lstcontactos=(ListView ) findViewById(R.id.lstcontactos);
        lstcontactos .setAdapter(new ArrayAdapter<String>(this, android.R.layout.simple_list_item_1, nombresContacto   ) ) ;

        lstcontactos.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                Intent intent=new Intent(MainActivity.this, Detalle.class) ;
                intent.putExtra(getResources().getString(R.string.pnombre),contactos.get(position).getNombre() );
                intent.putExtra(getResources().getString(R.string.ptelefono),contactos.get(position).getTelefono() );
                intent.putExtra(getResources().getString(R.string.pemail),contactos.get(position).getEmail() );
                intent.putExtra(getResources().getString(R.string.pdescripcion),contactos.get(position).getDescripcion() );
                startActivity(intent) ;





            }
        });


    }
}

Diseño
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:paddingLeft="20dp"
    android:paddingTop="16dp"
    android:paddingRight="16dp"
    android:paddingBottom="16dp"
    tools:context=".Detalle">

    <TextView
        android:id="@+id/tvnombre"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="40dp"
        android:textStyle="bold"
        tools:text="@string/nombre" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="20dp"
        android:orientation="horizontal"
        android:onClick="llamar"
        >

        <ImageView
            android:id="@+id/imgvtelefono"
            android:layout_width="48dp"
            android:layout_height="60dp"
            android:src="@mipmap/ic_launcher"
            app:srcCompat="@drawable/cel" />

        <TextView
            android:id="@+id/tvtelefono"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center_vertical"
            android:text="@string/telefono"
            android:textSize="20sp" />



    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="center_vertical"
        android:layout_marginTop="30dp"
        android:orientation="horizontal"
        android:onClick="enviaremail" >

        <ImageView
            android:id="@+id/img"
            android:layout_width="48dp"
            android:layout_height="60dp"
            android:src="@mipmap/ic_launcher_foreground"
            app:srcCompat="@drawable/correo"
            tools:srcCompat="@mipmap/ic_launcher_foreground" />

        <TextView
            android:id="@+id/tvemail"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/email"
            android:layout_gravity="center_vertical"/>
    </LinearLayout>


    <TextView
        android:id="@+id/tvdescripcion"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="center_vertical"
        android:layout_marginTop="30dp"
        android:orientation="horizontal"
        android:text="@string/descripcion" />
</LinearLayout>
