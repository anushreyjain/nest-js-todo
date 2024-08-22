DTO (Data Transfer Object) - 
- Can be defined using an interface or a class. (Class is Recomended)
The reason is that interface are a part of TS and are not preserved post compilation. Whereas classes are a part of JS and are prserved post compilation.
- It has behaviour of just storage, retrival, serialization and deserialization.
- it increase performance.
- can be useful for data validation.
- It is not a Model Defination, it just defines the shape of data for a specific case, for ex - create a task



Pipes - 
- Flow with pipe - 
    1. Success Case - 
HTTP Request ---- handler is identified ----- Pipe (Validate args) ---- If Success --------- handler is called  --------- HTTP Response(201)

    2. Failure Case - 
HTTP Request ---- handler is identified ----- Pipe (Validate args) ---- If Failed --------- BadRequestException is thrown  --------- HTTP Response(400)

- They are operated on the arguments to be processed by route handlers, just before the handler is called.
- They can perform data transformation or data validation.
- They can return data - either original or modified, which will be passed to route handlers.
- They can throw exceptions. Exception thrown will be handled by NESTJS and parsed into an error response.
- They can be asynchronous.

Default Pipes - 
1. Validation pipe - 
validate the data type of an entire object against a calss (goes well with DTOs). If any property cannot be mapped properly, validation will fail. It is a built-in validation pipe.

2. ParseIntPipe
By default, args are type of String. This pipe validates that an agrument is a number. If yes, the args is transferred into a Number and passed on to the handler.

Custom Pipe Implementation - 
- They are classed annotated with the @Injectable() decorator.
- They must have the PipeTransform generic interface. Therefor, every pipe must have a transform() method. This method will be called by NestJS to process the args.
- transform() method accepts two parameters :
    1. value : the value of the processed argument.
    2. metadata (optional) : an object containing metadata about the argument.
- whatever is returned from the transform() method will be passed on the route handler. Execption will be sent back to the client.

Pipes can be consumed in different ways

1. Handler-level pipes - 
They are defined on handler level, via the @UsePipes() decorator. such pipes will process all parameters for the incoming requests.

```
@Post()
@UsePipes(SomePipe)
createTask(@Body('description') description){
    body of handler
}
```


2. Parameter-level pipes - 
They are defined on parameter level, Only the specefic parameter for which the pipe has been specified will be processed.

```
@Post()
createTask(@Body('description' SomePipe) description){
    body of handler
}
```

3. Global pipes - 
They are defined at the application level and will be applied to any incoming request.

```
async function bootstrap(){
    const app = await NestFactory.create(ApplicationModule);
    app.usGlobalPipes(SomePipe);
    await app.listen(3000);
}
bootstrap()
```

