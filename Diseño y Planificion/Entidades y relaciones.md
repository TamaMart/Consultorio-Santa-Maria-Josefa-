## Sistema de gestión de insumos —
### Consultorio Santa María Josefa
 
Proyecto de la asignatura Taller Base de Datos (AIEP, NRC
PRO202) — Actividad de Aprendizaje + Servicio.
 
### Objetivos
1. Implementar un sistema de base de datos que permita
registrar el 100% de los insumos utilizados por box,
especialidad y profesional en el consultorio "Santa
María Josefa", eliminando el registro manual en papel
actualmente utilizado.
3. Generar reportes automatizados mediante consultas
SQL que permitan al consultorio identificar en cualquier
momento el consumo de insumos por box,
especialidad y profesional, reduciendo el desfase entre
el stock real y el stock registrado.
5. Implementar un mecanismo de alerta automática
dentro del sistema que notifique al personal del
consultorio y al proveedor cuando el stock de un
insumo alcance o esté por debajo del stock mínimo
definido, o cuando un insumo esté próximo a su fecha
de vencimiento, permitiendo la reposición o el retiro
oportuno antes de que se agote o venza.
### Entidades y atributos
Especialidad: id_especialidad (PK), nombre_especialidad
 
Box: id_box (PK), numero_box, id_especialidad (FK)
 
Personal: id_personal (PK), nombre, correo, telefono,
id_especialidad (FK)
 
Proveedor: id_proveedor (PK), nombre_proveedor, correo,
telefono
 
Categoria_Insumo: id_categoria (PK), nombre_categoria
 
Insumo: id_insumo (PK), nombre_insumo, id_categoria (FK),
unidad_medida, stock_actual, stock_minimo,
fecha_vencimiento, id_proveedor (FK)
 
Servicio: id_servicio (PK), id_box (FK), id_personal (FK),
fecha, motivo
 
Ingreso: id_ingreso (PK), id_proveedor (FK), id_personal (FK),
tipo_origen, fecha, observacion
 
Detalle_Servicio: id_servicio (PK, FK), id_insumo (PK, FK),
cantidad
 
Detalle_Ingreso: id_ingreso (PK, FK), id_insumo (PK, FK),
cantidad
 
Alerta: id_alerta (PK), id_insumo (FK), tipo_alerta, mensaje,
fecha_generada, estado
 
Canal_Comunicacion: id_canal (PK), nombre_canal
 
Notificacion: id_notificacion (PK), id_alerta (FK), id_canal (FK),
tipo_destinatario, id_personal (FK), id_proveedor (FK),
fecha_envio, estado_envio
 
### Modelo entidad-relación
```mermaid
erDiagram
  ESPECIALIDAD ||--o{ BOX : se_asigna_a
  ESPECIALIDAD ||--o{ PERSONAL : tiene
  PERSONAL ||--o{ SERVICIO : realiza
  BOX ||--o{ SERVICIO : ocurre_en
  PERSONAL ||--o{ INGRESO : registra
  PROVEEDOR ||--o{ INGRESO : entrega
  PROVEEDOR ||--o{ INSUMO : provee
  CATEGORIA_INSUMO ||--o{ INSUMO : clasifica
  SERVICIO ||--o{ DETALLE_SERVICIO : incluye
  INSUMO ||--o{ DETALLE_SERVICIO : es_usado_en
  INGRESO ||--o{ DETALLE_INGRESO : incluye
  INSUMO ||--o{ DETALLE_INGRESO : es_recibido_en
  INSUMO ||--o{ ALERTA : genera
  ALERTA ||--o{ NOTIFICACION : se_envia_mediante
  CANAL_COMUNICACION ||--o{ NOTIFICACION : usa
  PERSONAL ||--o{ NOTIFICACION : recibe
  PROVEEDOR ||--o{ NOTIFICACION : recibe
 
  ESPECIALIDAD {
    int id_especialidad PK "Identificador unico de la especialidad"
    string nombre_especialidad "Nombre de la especialidad medica"
  }
  BOX {
    int id_box PK "Identificador unico del box"
    string numero_box "Numero o codigo del box fisico"
    int id_especialidad FK "Especialidad asignada al box"
  }
  PERSONAL {
    int id_personal PK "Identificador unico del profesional"
    string nombre "Nombre completo del profesional"
    string correo "Correo de contacto"
    string telefono "Telefono de contacto"
    int id_especialidad FK "Especialidad del profesional"
  }
  PROVEEDOR {
    int id_proveedor PK "Identificador unico del proveedor"
    string nombre_proveedor "Nombre o razon social del proveedor"
    string correo "Correo de contacto del proveedor"
    string telefono "Telefono de contacto del proveedor"
  }
  CATEGORIA_INSUMO {
    int id_categoria PK "Identificador unico de la categoria"
    string nombre_categoria "Nombre de la categoria de insumo"
  }
  INSUMO {
    int id_insumo PK "Identificador unico del insumo"
    string nombre_insumo "Nombre del insumo"
    int id_categoria FK "Categoria a la que pertenece"
    string unidad_medida "Unidad en que se mide (unidad, caja, ml)"
    int stock_actual "Cantidad disponible en bodega"
    int stock_minimo "Cantidad minima antes de alertar"
    date fecha_vencimiento "Fecha de vencimiento del insumo"
    int id_proveedor FK "Proveedor habitual del insumo"
  }
  SERVICIO {
    int id_servicio PK "Identificador unico de la consulta"
    int id_box FK "Box donde se realizo la consulta"
    int id_personal FK "Profesional que realizo la consulta"
    date fecha "Fecha de la consulta"
    string motivo "Motivo de la consulta"
  }
  INGRESO {
    int id_ingreso PK "Identificador unico del ingreso"
    int id_proveedor FK "Proveedor que entrego el ingreso"
    int id_personal FK "Personal que registro el ingreso"
    string tipo_origen "Origen del ingreso, ej compra o donacion"
    date fecha "Fecha del ingreso"
    string observacion "Observaciones del ingreso"
  }
  DETALLE_SERVICIO {
    int id_servicio PK,FK "Consulta a la que pertenece el detalle"
    int id_insumo PK,FK "Insumo utilizado en la consulta"
    int cantidad "Cantidad de insumo utilizada"
  }
  DETALLE_INGRESO {
    int id_ingreso PK,FK "Ingreso al que pertenece el detalle"
    int id_insumo PK,FK "Insumo que se recibio"
    int cantidad "Cantidad de insumo recibida"
  }
  ALERTA {
    int id_alerta PK "Identificador unico de la alerta"
    int id_insumo FK "Insumo que genero la alerta"
    string tipo_alerta "Tipo, ej stock minimo o vencimiento"
    string mensaje "Mensaje descriptivo de la alerta"
    date fecha_generada "Fecha en que se genero la alerta"
    string estado "Estado de la alerta, ej pendiente o enviada"
  }
  CANAL_COMUNICACION {
    int id_canal PK "Identificador unico del canal"
    string nombre_canal "Nombre del canal, ej correo o whatsapp"
  }
  NOTIFICACION {
    int id_notificacion PK "Identificador unico de la notificacion"
    int id_alerta FK "Alerta que origino la notificacion"
    int id_canal FK "Canal por el que se envio"
    string tipo_destinatario "Tipo, ej personal o proveedor"
    int id_personal FK "Personal destinatario si corresponde"
    int id_proveedor FK "Proveedor destinatario si corresponde"
    date fecha_envio "Fecha de envio de la notificacion"
    string estado_envio "Estado del envio, ej enviado o fallido"
  }
```
 

