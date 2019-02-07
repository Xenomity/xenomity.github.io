---
title: "JAX-RS Annotation"
tags: [jax-rs, web-service]
date: 2010-12-27T16:18:18+09:00
---

@Path : Request URL  
@GET, @POST, @DELETE, @PUT : HTTP Method Mapping  
@Consumes : Response Data Format 정의 (HTTP Content-Type)  
@Produces : Request Data Format 정의 (HTTP Accept Header)  
@QueryParam : URI Query Parameter 획득  
@FormParam : Form Data 획득  
@PathParam : URI에 명시되어 있는 데이터 획득  
@CookieParam : Cookie Data 획득  
@HeaderParam : Request Header Data 획득  
@Context : Request Header, URI Inject Data 획득  
@DefaultValue : Default Value 정의  
@Encoded : URI Decoding 여부 설정

