Este proyecto es una prueba que debe cumplir un objetivo funcional segun este diseño:
  https://www.figma.com/design/RxextQHeFs98SQKdpTrahk/Pok%C3%A9dex?node-id=12-303&node-type=symbol&t=jUB6M70bplwRVHIa-0 
  
  para ello utilicé Vue(3) con el framework vuetify(3.6.11) en su version actual y estable 
  y como gestor de paquete de node pnpm ay que brinda un rendimiento mas eficiente y rapido al momento de compilar o levantar el host local
  como manejo de sintaxis complementaria al framework vuetify seleccione el esquema basado en typescript en la configuracion al momento de iniciar el proyecto ya que 
  el uso de Vue con TypeScript ayuda a detectar muchos errores comunes mediante análisis estático en el momento de la compilación. 
  Esto reduce la posibilidad de errores de ejecución en producción y también nos permite refactorizar código con mayor confianza en aplicaciones a gran escala
  
  para la parte de home fue muy sencilla solo agrege el logo de pikachu en representacion del diseño presentado, seguido del un contenido de texto sin mayor complicacion,
  al final un boton redireccionando a una lista basica con un buscador para buscar segun el nombre del pokemon que se desee tener una informacion mayor
  
  para la lista se consulto la api que trae una lista de n cantidad de pokemon segun el rango que deseemos, en este caso tambien use el localstorage para almacenar
  los nombres de los pokemons que vaya seleccionando como favoritos, con ello al final de obtener la lista tambien consultaba los datos del localstorage y  seteaba cuales 
  tenia ya marcado como favoritos y para la lista de favoritos solo consultaba la informacion del localstorage
  
  en el detalle use el nombre del pokemon para usar una sola funcion tanto para el detalle del pokemon en el modal como para buscar informacion de algun 
  pokemon en el campo del buscador (input search)
  
  
