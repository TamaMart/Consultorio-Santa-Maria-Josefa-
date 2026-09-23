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
Especialidad: id_especialidad (PK),
nombre_especialidad

Personal: id_personal (PK), nombre, id_especialidad
(FK)

Box: id_box (PK), numero_box, id_especialidad (FK)

Proveedor: id_proveedor (PK), nombre_proveedor,
contacto

Categoria_Insumo: id_categoria (PK),
nombre_categoria

Insumo: id_insumo (PK), nombre_insumo, id_categoria
(FK), unidad_medida, stock_actual, stock_minimo,
fecha_vencimiento, id_proveedor (FK)

Servicio: id_servicio (PK), id_box (FK), id_personal (FK),
id_insumo (FK), cantidad, fecha, motivo

Alerta: id_alerta (PK), id_insumo (FK), tipo_alerta,
fecha_generada

### Modelo entidad-relación
```mermaid
erDiagram
ESPECIALIDAD ||--o{ PERSONAL : tiene
ESPECIALIDAD ||--o{ BOX : se_asigna_a
PERSONAL ||--o{ SERVICIO : realiza
BOX ||--o{ SERVICIO : ocurre_en
INSUMO ||--o{ SERVICIO : es_usado_en
PROVEEDOR ||--o{ INSUMO : provee
CATEGORIA_INSUMO ||--o{ INSUMO : clasifica
INSUMO ||--o{ ALERTA : genera
ESPECIALIDAD {
int id_especialidad PK
string nombre_especialidad
}
PERSONAL {
int id_personal PK
string nombre
int id_especialidad FK
}
BOX {
int id_box PK
string numero_box
int id_especialidad FK
}
PROVEEDOR {
int id_proveedor PK
string nombre_proveedor
string contacto
}
CATEGORIA_INSUMO {
int id_categoria PK
string nombre_categoria
}
INSUMO {
int id_insumo PK
string nombre_insumo
int id_categoria FK
string unidad_medida
int stock_actual
int stock_minimo
date fecha_vencimiento
int id_proveedor FK
}
SERVICIO {
int id_servicio PK
int id_box FK
int id_personal FK
int id_insumo FK
int cantidad
date fecha
string motivo
}
ALERTA {
int id_alerta PK
int id_insumo FK
string tipo_alerta
date fecha_generada
}
```


## nuevo diagrama con correccion 
<img width="1472" height="1284" alt="image" src="https://github.com/user-attachments/assets/d523eea1-9ece-4ccd-9a0e-766a54640eef" />
