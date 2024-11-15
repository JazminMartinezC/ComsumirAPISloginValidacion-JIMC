# Estos son los pasos que se siguen hasta el momento de mostrar elementos en la tabla
## Reporte de practica 
#### Como primer paso lo que hacemos es crear el proyecto en la terminal mediante el comando de:
Run `ng new nombreProyecto`
#### luego de eso agregamos los componentes materials de angular por tanto se usa el comando de:
Run `ng @angular/material`
#### En este caso creare 2 componentes el cual el primero es el el componente 
##### ° component/user-list
##### ° service
![image](https://github.com/user-attachments/assets/eacda088-5712-4a3d-8ccc-7ff9d35c4927)

#### Una vez creado los componentes se accede a el componente service en el archivo user.service.ts
![image](https://github.com/user-attachments/assets/baf5788a-bf43-461f-8e37-d82f9aca6a1d)

#### En el componente de user-list.component.ts

#### UserListComponent

Este componente de Angular muestra una tabla con una lista de usuarios. Utiliza Angular Material para proporcionar funcionalidad de paginación, ordenamiento y filtrado.

#### Código del Componente

```typescript
import { AfterViewInit, Component, OnInit, viewChild } from '@angular/core';
import { MatPaginator } from '@angular/material/paginator';
import { MatSort } from '@angular/material/sort';
import { MatTableDataSource, MatTableModule } from '@angular/material/table';
import { UserService } from '../../services/user.service';
import { MatFormFieldModule } from '@angular/material/form-field';
import { MatInputModule } from '@angular/material/input';

@Component({
  selector: 'app-user-list',
  standalone: true,
  imports: [
    MatPaginator,MatSort,
    MatTableModule,MatFormFieldModule,
    MatInputModule
  ],
  templateUrl: './user-list.component.html',
  styleUrls: ['./user-list.component.css']
})
export class UserListComponent implements OnInit, AfterViewInit {
  displayedColumns: string[] = ['id', 'name', 'email', 'role'];
  dataSource = new MatTableDataSource<any>([]); // Inicializa con un arreglo vacío

  readonly paginator = viewChild.required(MatPaginator);
  readonly sort = viewChild.required(MatSort);

  constructor(private userService: UserService) {}

  ngOnInit(): void {
    // Llama al servicio para obtener los usuarios y asignarlos al dataSource
    this.userService.getUsers().subscribe((data: any[]) => {
      this.dataSource.data = data; // Asigna los datos obtenidos a dataSource
    });
  }

  ngAfterViewInit() {
    this.dataSource.paginator = this.paginator();
    this.dataSource.sort = this.sort();
  }

  applyFilter(event: Event) {
    const filterValue = (event.target as HTMLInputElement).value;
    this.dataSource.filter = filterValue.trim().toLowerCase();

    if (this.dataSource.paginator) {
      this.dataSource.paginator.firstPage();
    }
  }
}

```

#### En el componente de user-list en el archivo usere-list.component.html

# Tabla de Usuarios

Este código HTML muestra una tabla interactiva de usuarios con funcionalidades como filtrado, ordenamiento y paginación, utilizando Angular Material.

## Código del Componente HTML y CSS

### HTML

```html
<div class="container">
  <!-- Título de la página -->
  <h1 class="title">Tabla de Usuarios</h1>

  <!-- Filtro -->
  <mat-form-field class="mat-form-field">
    <mat-label>Filtrar</mat-label>
    <input matInput (keyup)="applyFilter($event)" placeholder="juan" #input />
  </mat-form-field>

  <!-- Tabla de usuarios -->
  <div class="mat-elevation-z8 table-container">
    <table mat-table [dataSource]="dataSource" matSort>
      <!-- ID Column -->
      <ng-container matColumnDef="id">
        <th mat-header-cell *matHeaderCellDef mat-sort-header> ID </th>
        <td mat-cell *matCellDef="let row">{{ row.id }}</td>
      </ng-container>

      <!-- Name Column -->
      <ng-container matColumnDef="name">
        <th mat-header-cell *matHeaderCellDef mat-sort-header> Nombre </th>
        <td mat-cell *matCellDef="let row">{{ row.name }}</td>
      </ng-container>

      <!-- Email Column -->
      <ng-container matColumnDef="email">
        <th mat-header-cell *matHeaderCellDef mat-sort-header> Email </th>
        <td mat-cell *matCellDef="let row">{{ row.email }}</td>
      </ng-container>

      <!-- Role Column -->
      <ng-container matColumnDef="role">
        <th mat-header-cell *matHeaderCellDef mat-sort-header> Rol </th>
        <td mat-cell *matCellDef="let row">{{ row.role }}</td>
      </ng-container>

      <tr mat-header-row *matHeaderRowDef="displayedColumns"></tr>
      <tr mat-row *matRowDef="let row; columns: displayedColumns;"></tr>

      <!-- Row shown when there is no matching data -->
      <tr class="mat-row" *matNoDataRow>
        <td class="mat-cell" colspan="4">No se encuentra en los "{{ input.value }}"</td>
      </tr>
    </table>
  </div>

  <!-- Paginador -->
  <mat-paginator [pageSizeOptions]="[5, 10, 25, 100]" aria-label="componente"></mat-paginator>
</div>
```

#### En el componente de app.component.ts se coloca el codigo de 
![image](https://github.com/user-attachments/assets/2749157b-094c-4c23-90d2-472bc8b77610)



## Pantalla final 
![image](https://github.com/user-attachments/assets/877c7a5e-23c7-4f21-b707-abe1a97c2999)
#### ° Se puede determinar la cantidad de usuarios que se mostraran en la tabla.
#### ° Paginación de la tabla.
#### ° Filtrado de un dato de la tabla.


## Preguntas en base al trabajo

##### ¿Qué hace el método getUsers en este servicio?

Funciona como el método que permitirá ver el flujo de los datos que están almacenados en el arreglo de any el cual tiene los datos que están en el API que se señaló.
##### ¿Por qué es necesario importar HttpClientModule?

Actualmente HttpClientModule no funciona por lo tanto se hace uso de HttpClient, este es importante importarlo para poder facilitar la comunicación entre la aplicación de Angular y el servidor las cuales dan resultados a las solicitudes de una API.
##### ¿Qué función cumple el método ngOnInit en el componente UserListComponent?

Llama al servicio para obtener los usuarios que están el arreglo el cual lo que hace es asignarlos a una variable para obtenerlos, una vez obtenida se reasigna y con este se puede trabajar.

##### ¿Para qué sirve el bucle *ngFor en Angular?

Sirve para generar el listado del contenido que tiene el arreglo que se pide en el servicio, actualmente se usa el @for el cual tiene la misma función
#### Preguntas para reflexionar

##### 1. ¿Qué ventajas tiene el uso de servicios en Angular para el consumo de APIs?
El uso de servicios facilita el uso de la llamadas y almacenamiento de los datos puesto que en este esta la información que puede ser utilizada en cualquier otro componente que se requiera llamar ya que también uso del componente de Injectable y este permite a relacionarse entre otros componentes.
##### 2. ¿Por qué es importante separar la lógica de negocio de la lógica de presentación?
Es importante por que uno se maneja diferente a otro ya que en este caso en la lógica de negocio se plantea el uso de las bases de datos y como este funge para cada uno de los componente o vista de usuario el cual es la lógica de presentación, en este caso el lado de la lógica de negocios se tiene que tener en cuenta factores como la seguridad de los datos, consistencia, integridad de los mismo puesto que en base a ellos un usuario final que hace uso de la interfaz de usuario tiene que tomar ciertas decisiones en base a lo que se muestra. integridad 


##### 3. ¿Qué otros tipos de datos o APIs podrías integrar en un proyecto como este? 
En este caso se pueden implementar diversos proyectos los cuales pueden ser APIS de un área empresarial, un espacio escolar los cuales también toman otros casos de APIS las cuales complementan el contenido del proyecto en el caso de un espacio escolar se pueden determinar la información de los estudiantes, los profesores, materias, horarios, pagos, evaluaciones, asistencias, etc. Las cuales impulsan a un proyecto mas elaborado.

# Para la validación de usuarios en el login en base a lo que existe en la API.
