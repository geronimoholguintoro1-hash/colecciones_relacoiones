package musica.app;

import java.util.ArrayList;

public class PruebaLista {
    public static void main(String[] args) {
        ArrayList<String> estilos = new ArrayList<>();

        estilos.add("Rock");
        estilos.add("Salsa");
        estilos.add("Vallenato");

        System.out.println(estilos.size());
        System.out.println(estilos.get(1));

        estilos.set(1, "Merengue");
        estilos.add(0, "Pop");

        System.out.println(estilos);

        estilos.remove("Rock");

        System.out.println(estilos.contains("Rock"));
        System.out.println(estilos.indexOf("Vallenato"));

        for (String estilo : estilos) {
            System.out.println(estilo.toUpperCase());
        }
    }
}




package musica.modelo;

public class Cancion {
    private String nombreCancion;
    private String nombreArtista;
    private int segundos;
    private static int totalCanciones = 0;

    public Cancion(String titulo, String artista, int duracion) {
        nombreCancion = titulo;
        nombreArtista = artista;
        segundos = duracion;
        totalCanciones++;
    }

    public String getTitulo() {
        return nombreCancion;
    }

    public void setTitulo(String titulo) {
        nombreCancion = titulo;
    }

    public String getArtista() {
        return nombreArtista;
    }

    public void setArtista(String artista) {
        nombreArtista = artista;
    }

    public int getDuracionSegundos() {
        return segundos;
    }

    public void setDuracionSegundos(int duracion) {
        segundos = duracion;
    }

    public static int getTotalCanciones() {
        return totalCanciones;
    }
}




package musica.modelo;

import java.util.ArrayList;

public class Playlist {
    private String nombreLista;
    private ArrayList<Cancion> listaCanciones;

    public static final int MAX_CANCIONES = 3;

    public Playlist(String nombre) {
        nombreLista = nombre;
        listaCanciones = new ArrayList<>();
    }

    public boolean agregar(Cancion cancion) {
        if (cancion == null) {
            return false;
        }

        if (listaCanciones.size() >= MAX_CANCIONES) {
            return false;
        }

        if (listaCanciones.contains(cancion)) {
            return false;
        }

        listaCanciones.add(cancion);
        return true;
    }

    public void mostrar() {
        System.out.println(nombreLista + ":");

        for (int i = 0; i < listaCanciones.size(); i++) {
            Cancion cancion = listaCanciones.get(i);

            System.out.println(
                (i + 1) + ". " + cancion.getTitulo()
                + " " + cancion.getArtista()
                + " (" + cancion.getDuracionSegundos() + " s)"
            );
        }
    }

    public int duracionTotal() {
        int totalSegundos = 0;

        for (Cancion cancion : listaCanciones) {
            totalSegundos += cancion.getDuracionSegundos();
        }

        return totalSegundos;
    }

    public Cancion buscar(String titulo) {
        for (Cancion cancion : listaCanciones) {
            if (cancion.getTitulo().equalsIgnoreCase(titulo)) {
                return cancion;
            }
        }

        return null;
    }

    public int eliminarArtista(String artista) {
        int eliminadas = 0;

        for (int i = listaCanciones.size() - 1; i >= 0; i--) {
            Cancion cancion = listaCanciones.get(i);

            if (cancion.getArtista().equalsIgnoreCase(artista)) {
                listaCanciones.remove(i);
                eliminadas++;
            }
        }

        return eliminadas;
    }
}




package musica.app;

import musica.modelo.Cancion;
import musica.modelo.Playlist;

public class Reproductor {
    public static void main(String[] args) {
        Cancion cancion1 =
            new Cancion("Lluvia en Manizales", "Nevado", 200);
        Cancion cancion2 =
            new Cancion("Bajo el guadual", "Café Trío", 245);
        Cancion cancion3 =
            new Cancion("Noche en Chipre", "Nevado", 185);
        Cancion cancion4 =
            new Cancion("Río Chinchiná", "Nevado", 230);

        Playlist favoritas = new Playlist("Favoritas");

        favoritas.agregar(cancion1);
        favoritas.agregar(cancion2);
        favoritas.agregar(cancion3);

        System.out.println(favoritas.agregar(cancion1));

        favoritas.mostrar();
        System.out.println(favoritas.duracionTotal());

        Playlist viaje = new Playlist("Viaje");

        viaje.agregar(cancion1);
        viaje.agregar(cancion2);

        System.out.println(favoritas.eliminarArtista("Nevado"));

        favoritas.mostrar();
        viaje.mostrar();

        System.out.println(
            favoritas.buscar("bajo el guadual").getArtista()
        );

        System.out.println(favoritas.buscar("Lluvia en Manizales"));

        System.out.println(viaje.agregar(cancion3));
        System.out.println(viaje.agregar(cancion4));

        System.out.println(Cancion.getTotalCanciones());
        System.out.println(Playlist.MAX_CANCIONES);
    }
}




package parqueadero.modelo;

public class Puesto {
    private int numeroPuesto;
    private String placaVehiculo;

    Puesto(int numero) {
        numeroPuesto = numero;
        placaVehiculo = null;
    }

    public int getNumero() {
        return numeroPuesto;
    }

    public String getPlaca() {
        return placaVehiculo;
    }

    public boolean estaLibre() {
        return placaVehiculo == null;
    }

    public void ocupar(String placa) {
        placaVehiculo = placa;
    }

    public void liberar() {
        placaVehiculo = null;
    }
}




package parqueadero.modelo;

import java.util.ArrayList;

public class Parqueadero {
    private String nombreParqueadero;
    private ArrayList<Puesto> espacios;
    private static int ingresosTotales = 0;

    public Parqueadero(String nombre, int cantidad) {
        nombreParqueadero = nombre;
        espacios = new ArrayList<>();

        for (int i = 1; i <= cantidad; i++) {
            espacios.add(new Puesto(i));
        }
    }

    public static int getIngresosTotales() {
        return ingresosTotales;
    }

    public void ingresar(String placa) {
        for (Puesto espacio : espacios) {
            if (placa.equals(espacio.getPlaca())) {
                System.out.println(placa + " ya está adentro");
                return;
            }
        }

        for (Puesto espacio : espacios) {
            if (espacio.estaLibre()) {
                espacio.ocupar(placa);
                ingresosTotales++;

                System.out.println(
                    placa + " en puesto " + espacio.getNumero()
                );
                return;
            }
        }

        System.out.println(placa + ": parqueadero lleno");
    }

    public boolean salir(String placa) {
        for (Puesto espacio : espacios) {
            if (placa.equals(espacio.getPlaca())) {
                espacio.liberar();
                return true;
            }
        }

        return false;
    }

    public int puestosLibres() {
        int disponibles = 0;

        for (Puesto espacio : espacios) {
            if (espacio.estaLibre()) {
                disponibles++;
            }
        }

        return disponibles;
    }
}




package parqueadero.app;

import parqueadero.modelo.Parqueadero;

public class Main {
    public static void main(String[] args) {
        Parqueadero parqueaderoCentro =
            new Parqueadero("Centro", 2);
        Parqueadero parqueaderoNorte =
            new Parqueadero("Norte", 3);

        parqueaderoCentro.ingresar("ABC123");
        parqueaderoCentro.ingresar("ABC123");
        parqueaderoCentro.ingresar("XYZ789");
        parqueaderoCentro.ingresar("JKL456");

        System.out.println(parqueaderoCentro.salir("ABC123"));

        parqueaderoCentro.ingresar("JKL456");
        parqueaderoNorte.ingresar("MNO321");

        System.out.println(parqueaderoCentro.puestosLibres());
        System.out.println(parqueaderoNorte.puestosLibres());

        System.out.println(Parqueadero.getIngresosTotales());
    }
}
