# Async / Await en C#

El uso de `async` y `await` simplifica el trabajo con tareas asíncronas.

## 🔍 Ejemplo básico

[Ver en SharpLab](https://sharplab.io/#gist:YYYYYYYY)

```csharp
static async Task Main()
{
    await PrintMessage();
}

static async Task PrintMessage()
{
    await Task.Delay(1000);
    Console.WriteLine("Hola desde async/await");
}
```
