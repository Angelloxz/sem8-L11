package com.example.demo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

import java.lang.String;


@Controller // This means that this class is a Controller
@RequestMapping(path="/")

public class MainController {
    @Autowired
    private CursoRepository Repository;

    @GetMapping(path="/")
    public @ResponseBody String Home() {return "A123456 - Angello Rodriguez Cansaya";}

    @GetMapping(path="/codigo")
    public @ResponseBody String codigo() {return "A123456";}

    @GetMapping(path="/nombre-completo")
    public @ResponseBody String nombre() {return "Angello Rodriguez Cansaya";}


    @PostMapping(path="/api/curso/nuevo") // POST http://localhost:8080/demo/add
    public @ResponseBody String addNewUser (@RequestParam String nombre
      , @RequestParam String creditos) {
    // @ResponseBody means the returned String is the response, not a view name
    // @RequestParam means it is a parameter from the GET or POST request

    Curso n = new Curso();
    n.setNombre(nombre);
    n.setCreditos(creditos);
    Repository.save(n);
    return "Curso Guardado";
  }

  @GetMapping(path="/api/curso/listar") // GET http://localhost:8080/demo/all
  public @ResponseBody Iterable<Curso> getAllUsers() {
    // This returns a JSON or XML with the users
    return Repository.findAll();
  }

  @DeleteMapping(path="/api/curso/eliminar")
  public @ResponseBody String deleteUser(@RequestParam Integer id) {
    Curso user = Repository.findById(id).orElse(null);
    if (user != null) {
      Repository.delete(user);
      return "Deleted";
    }
    return "Not found";
  }
    
}
