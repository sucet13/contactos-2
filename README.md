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
