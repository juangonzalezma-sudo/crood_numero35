# crood_numero35
hola, mi nombre es juan diego gonzalez marin 
package com.ejemplo.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.*;
import org.springframework.stereotype.*;
import org.springframework.data.jpa.repository.JpaRepository;
import jakarta.persistence.*;
import org.springframework.beans.factory.annotation.Autowired;
import java.util.List;

@SpringBootApplication
public class DemoApplication {
public static void main(String[] args) {
SpringApplication.run(DemoApplication.class, args);  
}
}

// Entidad
@Entity
class Usuario {
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
private String nombre;
private String email;

    // Getters y setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}

// Repositorio
interface UsuarioRepository extends JpaRepository<Usuario, Long> {}

// Servicio
@Service
class UsuarioService {
@Autowired
private UsuarioRepository usuarioRepository;

    public List<Usuario> listarUsuarios() {
        return usuarioRepository.findAll();
    }
    public Usuario crearUsuario(Usuario usuario) {
        return usuarioRepository.save(usuario);
    }
    public Usuario obtenerUsuario(Long id) {
        return usuarioRepository.findById(id).orElse(null);
    }
    public Usuario actualizarUsuario(Long id, Usuario usuario) {
        usuario.setId(id);
        return usuarioRepository.save(usuario);
    }
    public void eliminarUsuario(Long id) {
        usuarioRepository.deleteById(id);
    }
}

// Controlador
@RestController
@RequestMapping("/usuarios")
class UsuarioController {
@Autowired
private UsuarioService usuarioService;

    @GetMapping
    public List<Usuario> listar() {
        return usuarioService.listarUsuarios();
    }
    @PostMapping
    public Usuario crear(@RequestBody Usuario usuario) {
        return usuarioService.crearUsuario(usuario);
    }
    @GetMapping("/{id}")
    public Usuario obtener(@PathVariable Long id) {
        return usuarioService.obtenerUsuario(id);
    }
    @PutMapping("/{id}")
    public Usuario actualizar(@PathVariable Long id, @RequestBody Usuario usuario) {
        return usuarioService.actualizarUsuario(id, usuario);
    }
    @DeleteMapping("/{id}")
    public void eliminar(@PathVariable Long id) {
        usuarioService.eliminarUsuario(id);
    }
}
package com.ejemplo.biblioteca;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.*;
import org.springframework.stereotype.*;
import org.springframework.data.jpa.repository.JpaRepository;
import jakarta.persistence.*;
import org.springframework.beans.factory.annotation.Autowired;
import java.util.List;

@SpringBootApplication
public class BibliotecaApplication {
public static void main(String[] args) {
SpringApplication.run(BibliotecaApplication.class, args);
}
}

// Entidad
@Entity
class Libro {
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
private String titulo;
private String autor;
private int anioPublicacion;
private boolean disponible;

    // Getters y setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getTitulo() { return titulo; }
    public void setTitulo(String titulo) { this.titulo = titulo; }
    public String getAutor() { return autor; }
    public void setAutor(String autor) { this.autor = autor; }
    public int getAnioPublicacion() { return anioPublicacion; }
    public void setAnioPublicacion(int anioPublicacion) { this.anioPublicacion = anioPublicacion; }
    public boolean isDisponible() { return disponible; }
    public void setDisponible(boolean disponible) { this.disponible = disponible; }
}

// Repositorio
interface LibroRepository extends JpaRepository<Libro, Long> {
List<Libro> findByAutor(String autor);
}

// Servicio
@Service
class LibroService {
@Autowired
private LibroRepository libroRepository;

    public List<Libro> listarLibros(String autor) {
        if (autor != null && !autor.isEmpty()) {
            return libroRepository.findByAutor(autor);
        }
        return libroRepository.findAll();
    }
    public Libro crearLibro(Libro libro) {
        return libroRepository.save(libro);
    }
    public Libro obtenerLibro(Long id) {
        return libroRepository.findById(id).orElse(null);
    }
    public Libro actualizarLibro(Long id, Libro libro) {
        libro.setId(id);
        return libroRepository.save(libro);
    }
    public void eliminarLibro(Long id) {
        libroRepository.deleteById(id);
    }
}

// Controlador
@RestController
@RequestMapping("/libros")
class LibroController {
@Autowired
private LibroService libroService;

    @GetMapping
    public List<Libro> listar(@RequestParam(required = false) String autor) {
        return libroService.listarLibros(autor);
    }
    @PostMapping
    public Libro crear(@RequestBody Libro libro) {
        return libroService.crearLibro(libro);
    }
    @GetMapping("/{id}")
    public Libro obtener(@PathVariable Long id) {
        return libroService.obtenerLibro(id);
    }
    @PutMapping("/{id}")
    public Libro actualizar(@PathVariable Long id, @RequestBody Libro libro) {
        return libroService.actualizarLibro(id, libro);
    }
    @DeleteMapping("/{id}")
    public void eliminar(@PathVariable Long id) {
        libroService.eliminarLibro(id);
    }
}
