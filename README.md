# **Proyecto: Almacenamiento de Documentos con MinIO y Go**

Este proyecto utiliza **MinIO**, un servidor de almacenamiento compatible con S3, para gestionar documentos localmente en un contenedor Docker. La API en Go se conecta a MinIO para cargar y recuperar documentos.

---

## **Configuración Inicial**

### **1. Descargar la Imagen de MinIO**

Ejecuta el siguiente comando para descargar la imagen oficial de MinIO desde Docker Hub:

```bash
docker pull minio/minio
```

### **2. Ejecutar Minio en un contenedor de docker**

usa este comando para correr el contenedor de MinIo:

```bash
docker run -d --name minio \
  -p 9000:9000 \
  -p 9001:9001 \
  -e MINIO_ROOT_USER=admin \
  -e MINIO_ROOT_PASSWORD=admin123 \
  minio/minio server /data --console-address ":9001"
```

- Parametros
 -- -p 9000:9000 : expone el puerto para la API de MinIO (igualemente seremos redirigidos hacia la consola).
 -- -p 9001:9001 : expone el puerto para la consola de MinIO en donde podremos manejar todos archivos con un UI.
 -- MINIO_ROOT_USER=admin: Usuario administrador.
 -- MINIO_ROOT_PASSWORD=admin123: Contraseña del administrador.
 -- minio/minio server /data: Define el directorio /data como almacenamiento.

### **3. Configurar Proyecto Go**

Instalar el **SDK** de MinIo:

```bash
go get github.com/minio/minio-go/v7
```
**Ejemplo de utilización**

```bash
package main

import (
	"context"
	"fmt"
	"log"

	"github.com/minio/minio-go/v7"
	"github.com/minio/minio-go/v7/pkg/credentials"
)

func main() {
	// Configuración de MinIO
	endpoint := "localhost:9000"
	accessKeyID := "admin"
	secretAccessKey := "admin123"
	useSSL := false

	// Crear cliente de MinIO
	minioClient, err := minio.New(endpoint, &minio.Options{
		Creds:  credentials.NewStaticV4(accessKeyID, secretAccessKey, ""),
		Secure: useSSL,
	})
	if err != nil {
		log.Fatalf("Error creando el cliente de MinIO: %v", err)
	}

	fmt.Println("Conexión exitosa a MinIO")
}
```

