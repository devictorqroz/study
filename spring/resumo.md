Exemplo das camadas:
GameController
GameService
GameRepository


###-----
#Controller
- porta de entrada do backend
- disponibiliza a API, a interface do backend.
  @Controller
  @RestController
  @RequestMapping(value = "/games") -> mapeamento de recurso
  @GetMapping no metodo.

-> injeta um Service


#####-------
#conceito de DTO
- não é mapeado com o banco
- só precisa de getters
- é o retorno da camada service para o controller, obtido do banco pela camada de acesso a dados.

------------------------


###-------------
#Service
- componente responsável por implementar lógica de negócio
- lógica de negócio: regras de negócio

-> injeta um repository
@Autowired

-> devolve um DTO
-------------------------


######
#Repository
-objeto de acesso a dados
- o objeto responsavel por fazer consulta ao banco de dados
  Trabalhando orientado a dominio
  Object bean/pojo
  -> "Object"Repository
  -> extends JpaRepository<"TypeObject", "Type ID">

-> Repository devolve uma entidade

#conceito de entidade
@entity
@table
@id
@GeneratedValue(strategy = GenerationType.IDENTITY)
@column
@Column(columnDefinition = "TEXT")

-------------------------------

###Annotations
@entity
- parte do JPA (Java Persistence API)
- mapeamento de classes com tabelas do banco de dados.
- é usada para indicar que uma classe Java representa uma entidade no banco de dados.





@Controller
@RestController
@RequestMapping
@GetMapping


@Configuration
@Component
@PathVariable
@PostMapping
@RequestBody
@DeleteMapping
@Transactional
@ComponentScan