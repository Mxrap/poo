// Classe Musica.java
public class Musica {
    String titulo;
    String artista;
    int duracaoSegundos;

    public Musica(String titulo, String artista, int duracaoSegundos) {
        this.titulo = titulo;
        this.artista = artista;
        this.duracaoSegundos = duracaoSegundos;
    }

    public void tocar() {
        System.out.println("Tocando: " + titulo + " - " + artista);
    }

    public String toString() {
        return titulo + " - " + artista + " (" + duracaoSegundos + "s)";
    }
}

// Classe NoDuplo.java
public class NoDuplo {
    Musica musica;
    NoDuplo anterior;
    NoDuplo proximo;

    public NoDuplo(Musica musica) {
        this.musica = musica;
        this.anterior = null;
        this.proximo = null;
    }
}

// Classe ListaMusicas.java
public class ListaMusicas {
    private NoDuplo inicio;
    private NoDuplo fim;
    private NoDuplo atual;

    public ListaMusicas() {
        this.inicio = null;
        this.fim = null;
        this.atual = null;
    }

    public void adicionarMusica(Musica musica) {
        NoDuplo novo = new NoDuplo(musica);
        if (inicio == null) {
            inicio = fim = novo;
        } else {
            fim.proximo = novo;
            novo.anterior = fim;
            fim = novo;
        }
    }

    public void removerMusica(String titulo) {
        NoDuplo temp = inicio;
        while (temp != null) {
            if (temp.musica.titulo.equalsIgnoreCase(titulo)) {
                if (temp.anterior != null) temp.anterior.proximo = temp.proximo;
                else inicio = temp.proximo;
                if (temp.proximo != null) temp.proximo.anterior = temp.anterior;
                else fim = temp.anterior;
                System.out.println("Removido: " + temp.musica.titulo);
                return;
            }
            temp = temp.proximo;
        }
        System.out.println("Música não encontrada: " + titulo);
    }

    public void listarMusicas() {
        NoDuplo temp = inicio;
        while (temp != null) {
            System.out.println(temp.musica);
            temp = temp.proximo;
        }
    }

    public void tocarAtual() {
        if (atual != null) atual.musica.tocar();
        else System.out.println("Nenhuma música selecionada.");
    }

    public void tocarProxima() {
        if (atual == null) atual = inicio;
        else if (atual.proximo != null) atual = atual.proximo;
        tocarAtual();
    }

    public void tocarAnterior() {
        if (atual != null && atual.anterior != null) {
            atual = atual.anterior;
            tocarAtual();
        } else {
            System.out.println("Sem música anterior.");
        }
    }
}

// Classe Main.java (para testar)
public class Main {
    public static void main(String[] args) {
        ListaMusicas playlist = new ListaMusicas();

        playlist.adicionarMusica(new Musica("Shape of You", "Ed Sheeran", 240));
        playlist.adicionarMusica(new Musica("Bohemian Rhapsody", "Queen", 354));
        playlist.adicionarMusica(new Musica("Blinding Lights", "The Weeknd", 200));

        System.out.println("Playlist:");
        playlist.listarMusicas();

        System.out.println("\nTocando músicas:");
        playlist.tocarProxima();
        playlist.tocarProxima();
        playlist.tocarAnterior();

        System.out.println("\nRemover música:");
        playlist.removerMusica("Blinding Lights");
        playlist.listarMusicas();
    }
}
