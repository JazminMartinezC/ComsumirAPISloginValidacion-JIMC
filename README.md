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

Este código HTML muestra una tabla interactiva de usuarios con funcionalidades como filtrado, ordenamiento y paginación, utilizando Angular Material.ademas de tener en la parte superior un menu de opciones.
el cual tiene un desplegable de opciones,

## Código del Componente HTML y CSS

### HTML

```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Mi Sitio Angular</title>
      <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500&display=swap" rel="stylesheet">
      <link href="https://cdn.jsdelivr.net/npm/@angular/material@15.0.0/prebuilt-themes/indigo-pink.css" rel="stylesheet">
      <style>
        /* Configuración de imagen de fondo */
        body, html {
          margin: 0;
          padding: 0;
          height: 100%;
          font-family: Arial, sans-serif;
          background-image: url('fondo.jpg');
          background-size: cover;
          background-position: center;
          color: white;
        }
    
        /* Estilo de la barra de navegación */
        .navbar {
          display: flex;
          justify-content: space-between;
          align-items: center;
          padding: 15px 20px;
          background-color: rgba(0, 0, 0, 0.7); /* Fondo semitransparente */
        }
    
        .navbar .logo {
          font-size: 24px;
          font-weight: bold;
          color: #ffffff;
          text-decoration: none;
        }
    
        .navbar .menu {
          list-style: none;
          display: flex;
          gap: 20px;
        }
    
        .navbar .menu li {
          display: inline;
        }
    
        .navbar .menu li a {
          color: #ffffff;
          text-decoration: none;
          font-size: 18px;
          transition: color 0.3s;
        }
    
        .navbar .menu li a:hover {
          color: #ffecb7; /* Cambia el color al pasar el cursor */
        }
    
        /* Contenedor principal */
        .main-container {
          display: flex;
          flex-direction: column;
          align-items: center;
          justify-content: center;
          padding: 20px;
        }
    
        .welcome-container {
          text-align: center;
          background-color: rgba(0, 0, 0, 0.6);
          padding: 20px;
          border-radius: 10px;
          margin-bottom: 30px;
        }
    
        .welcome-container h1 {
          font-size: 48px;
          color: #ffffff;
          margin: 0;
        }
    
        .welcome-container p {
          font-size: 24px;
          color: #ffffff;
          margin: 10px 0 20px;
        }
    
        .welcome-button {
          display: inline-block;
          padding: 10px 20px;
          font-size: 18px;
          color: #ffffff;
          background-color: #007bff;
          border: none;
          border-radius: 5px;
          text-decoration: none;
          cursor: pointer;
          transition: background-color 0.3s;
        }
    
        .welcome-button:hover {
          background-color: #0056b3;
        }
    
        /* Estilo para la tabla */
        .menu-bar {
          display: flex;
          gap: 15px;
          margin-bottom: 20px;
        }
    
        .table-container {
          width: 100%;
        }
    

    
        .mat-form-field {
          width: 100%;
          max-width: 500px;
          margin-bottom: 20px;
        }
      </style>
    </head>
    <body>
    
      <!-- Barra de menú principal -->
      <nav class="navbar">
        <a href="#" class="logo">Mi Sitio</a>

                 <!-- Barra de menú interactiva -->
        <div class="menu-bar">
          <button mat-button [matMenuTriggerFor]="menu1" class="color">Menú 1</button>
          <mat-menu #menu1="matMenu" yPosition="above">
            <button mat-menu-item>Mi información</button>
            <button mat-menu-item>Escuela</button>
          </mat-menu>
    
          <button mat-button [matMenuTriggerFor]="menu2" class="color">Menú 2</button>
          <mat-menu #menu2="matMenu" yPosition="above">
            <button mat-menu-item>Mi información</button>
            <button mat-menu-item>Escuela</button>
          </mat-menu>
    
          <button mat-button [matMenuTriggerFor]="menu3" class="color">Menú 3</button>
          <mat-menu #menu3="matMenu" yPosition="above">
            <button mat-menu-item>Mi información</button>
            <button mat-menu-item>Escuela</button>
          </mat-menu>
    
          <button mat-button [matMenuTriggerFor]="menu4" class="color">Menú 4</button>
          <mat-menu #menu4="matMenu" yPosition="above">
            <button mat-menu-item>Mi información</button>
            <button mat-menu-item>Escuela</button>
          </mat-menu>
        </div>     
      </nav>
    
      <!-- Contenido principal -->
      <div class="main-container">
    
        <!-- Pantalla de bienvenida -->
        <div class="welcome-container">
          <h1>Bienvenido a mi página en Angular</h1>
                </div>    
        <!-- Tabla de usuarios -->
        <h1 class="title">Tabla de Usuarios</h1>
        <mat-form-field class="mat-form-field">
          <mat-label>Filtrar</mat-label>
          <input matInput (keyup)="applyFilter($event)" placeholder="juan" #input />
        </mat-form-field>
    
        <div class="mat-elevation-z8 table-container">
          <table mat-table [dataSource]="dataSource" matSort>
            <ng-container matColumnDef="id">
              <th mat-header-cell *matHeaderCellDef mat-sort-header> ID </th>
              <td mat-cell *matCellDef="let row">{{ row.id }}</td>
            </ng-container>
    
            <ng-container matColumnDef="name">
              <th mat-header-cell *matHeaderCellDef mat-sort-header> Nombre </th>
              <td mat-cell *matCellDef="let row">{{ row.name }}</td>
            </ng-container>
    
            <ng-container matColumnDef="email">
              <th mat-header-cell *matHeaderCellDef mat-sort-header> Email </th>
              <td mat-cell *matCellDef="let row">{{ row.email }}</td>
            </ng-container>
    
            <ng-container matColumnDef="role">
              <th mat-header-cell *matHeaderCellDef mat-sort-header> Rol </th>
              <td mat-cell *matCellDef="let row">{{ row.role }}</td>
            </ng-container>
    
            <tr mat-header-row *matHeaderRowDef="displayedColumns"></tr>
            <tr mat-row *matRowDef="let row; columns: displayedColumns;"></tr>
            <tr class="mat-row" *matNoDataRow>
              <td class="mat-cell" colspan="4">No se encuentra en los "{{ input.value }}"</td>
            </tr>
          </table>
        </div>
    
        <mat-paginator [pageSizeOptions]="[5, 10, 25, 100]" aria-label="componente"></mat-paginator>
        <br>

        <a href="#" class="welcome-button">Regresar a Login</a>

      </div>
      
    </body>
    </html>

```

#### En el componente de app.component.ts se coloca el codigo de 
![image](https://github.com/user-attachments/assets/2749157b-094c-4c23-90d2-472bc8b77610)



## Pantalla final 
![image](https://github.com/user-attachments/assets/76f50572-7514-4479-8443-5e0d4e9283b1)

#### ° Se puede determinar la cantidad de usuarios que se mostraran en la tabla.
#### ° Paginación de la tabla.
#### ° Filtrado de un dato de la tabla.

# Para la validación de usuarios en el login en base a lo que existe en la API.

#### se crea un nuevo componente el cual tiene el nombre de login
Run `ng generate component login`

#### Una vez creado lo que se hace es que en el archivo de login.component. ts se le agrega el codigo para poder validar los datos que se piden en los fild de correo y contraseña  para acceder al siguiente componente 
``` typescript
import { Component } from '@angular/core';
import { Router } from '@angular/router';
import { MatCardModule } from '@angular/material/card';
import { MatTabsModule } from '@angular/material/tabs';
import { MatInputModule } from '@angular/material/input';
import { MatButtonModule } from '@angular/material/button';
import { MatIconModule } from '@angular/material/icon';
import { FormsModule } from '@angular/forms';
import { MatSnackBarModule } from '@angular/material/snack-bar';
import { CommonModule } from '@angular/common'; 
import { UserService } from '../services/user.service';          // Importar el servicio
import { MatSnackBar } from '@angular/material/snack-bar';
import {MatStepperModule} from '@angular/material/stepper';
@Component({
  selector: 'app-login',
  standalone: true,
  imports: [
    CommonModule,      
    MatCardModule,
    MatTabsModule,
    MatInputModule,
    MatButtonModule,
    MatIconModule,
    FormsModule,
    MatSnackBarModule, MatStepperModule
  ],
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent {
  email: string = '';
  password: string = '';
  remail: string = '';
  rpassword: string = '';
  rconfirmPassword: string = '';

  constructor(private router: Router, private userService: UserService, private snackBar: MatSnackBar) {}

  iniciarPagina() {
    // Validación de los campos de correo y contraseña
    if (this.email && this.password) {
      // Llamada al servicio para obtener los usuarios
      this.userService.getUsers().subscribe(
        (users) => {
          // Buscar el usuario que coincida con el correo y la contraseña
          const user = users.find(u => u.email === this.email && u.password === this.password);
          
          if (user) {
            // Si el usuario es válido, navegar al dashboard
            this.router.navigate(['/user-list']);
          } else {
            // Si no se encuentra un usuario que coincida, mostrar mensaje de error
            this.snackBar.open('Credenciales incorrectas', 'Cerrar', { duration: 3000 });
          }
        },
        (error) => {
          // Manejo de error en caso de que falle la llamada a la API
          console.error('Error al obtener los usuarios', error);
          this.snackBar.open('Error al obtener los usuarios', 'Cerrar', { duration: 3000 });
        }
      );
    } else {
      // Si no se completan los campos
      this.snackBar.open('Por favor complete los campos', 'Cerrar', { duration: 3000 });
    }
  }
}
```
 #### En el componente de login.component.html se realizo todo la estructura para poder ingresar los datos necesarios para validar los cuales son correo y contraseña.
 ```html
<style>
    div{
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-size: cover;
            background-color: rgb(255, 222, 112);
            background-image: url("fondo.jpg");
            background-position: center;
            padding: 20px;
            box-sizing: border-box;
          }
          .btn {
            margin-top: 20px;
            padding: 12px 24px;
            font-size: 18px;
            color: white;
            background-color: #007bff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
            align-items: center;
          }

          mat-card-title
          {
            align-items: center;
            align-content: center;
          }

          mat-form-field
          {
            color: black;
            background-color: rgb(255, 255, 255);
        }
          </style>
<div >
    
      <mat-card class="example-card">
        <mat-card-header>
          <mat-card-title>Iniciar sesion - Registro </mat-card-title>
        </mat-card-header>
        <mat-card-content>
          <mat-tab-group dynamicHeight>
            
            <mat-tab label="Iniciar Sesión">
              <mat-form-field class="example-full-width">
                <mat-label>Correo</mat-label>
                <input type="email" matInput placeholder="Email" [(ngModel)]="email" required>
                <mat-icon matSuffix>email</mat-icon>
              </mat-form-field>
              <br>
              <mat-form-field>
                <mat-label>Contraseña</mat-label>
                <input type="password" matInput placeholder="Password" [(ngModel)]="password" required>
                <mat-icon matSuffix>vpn_key</mat-icon>
              </mat-form-field>
              <br>
              <button class="btn" (click)="iniciarPagina()">Login</button>
            </mat-tab>

            <mat-tab label="Register">
              <mat-form-field class="example-full-width">
                <mat-label>Correo</mat-label>
                <input type="email" matInput placeholder="Email" [(ngModel)]="remail" required>
                <mat-icon matSuffix>email</mat-icon>
              </mat-form-field>
              <br>
              <mat-form-field class="example-full-width">
                <mat-label>Contraseña</mat-label>
                <input type="password" matInput placeholder="Password" [(ngModel)]="rpassword" required>
                <mat-icon matSuffix>vpn_key</mat-icon>
              </mat-form-field>
              <br>
              <mat-form-field class="example-full-width">
                <mat-label>Confirmar contraseña</mat-label>
                <input type="password" matInput placeholder="Confirm Password" [(ngModel)]="rconfirmPassword" required>
                <mat-icon matSuffix>vpn_key</mat-icon>
              </mat-form-field>
              <br>
              <button class="btn" (click)="iniciarPagina()">Register</button>
            </mat-tab>
          </mat-tab-group>
        </mat-card-content>
      </mat-card>
    </div>
```
#### En el el archivo de app.routes.ts se realiza la codificación para poder navegar entre los componentes. Por tanto se crean las rutas que se mostraran durante el uso.
```typescript
import { Routes } from '@angular/router';
import { LoginComponent } from './login/login.component';
import { UserListComponent } from './components/user-list/user-list.component';
export const routes: Routes = [
    {
        path: '', redirectTo: '/login',pathMatch:'full'
    },
    {
        path: 'login', component:LoginComponent
    },
    {
        path: 'user-list', component:UserListComponent
    },
];

```

### Resultado de la ejecución 
#### Login
![image](https://github.com/user-attachments/assets/74b65dd3-f439-492d-a15b-33af9ff82f26)


### muestra el siguiente componente 
![image](https://github.com/user-attachments/assets/4c57ecc8-dbdf-4b76-8d79-f72fdc6c0da5)

