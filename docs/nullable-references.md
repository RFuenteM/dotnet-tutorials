# Referencias nulas en C\#

Desde C# 8, el compilador incluye análisis de nulabilidad (`nullable reference types`).

---

## 🔍 Ejemplo con advertencia

El siguiente código genera `CS8602` (*Dereference of a possibly null reference*):

```csharp
#nullable enable

string? text = null;
Console.WriteLine(text.Length); // ⚠️ Posible null
```

✅ Solución
```csharp
Console.WriteLine(text?.Length ?? 0);
```

## 🔍 Ejemplo real en Driver

El siguiente código genera `CS8600` (*Converting null literal or possible null value to non-nullable type.*):
```csharp
int configTimeMin = 10;
IntParameter configTimeMinParam = parameter.Parent.GetParam("CONFIG_MARGIN_TIME") as IntParameter; // ⚠️ Posible null
if (configTimeMinParam != null) configTimeMin = configTimeMinParam.Value;
```

💡 Eliminación de la advertencia
```csharp
IntParameter? configTimeMinParam = parameter.Parent.GetParam("CONFIG_MARGIN_TIME") as IntParameter;
```

💡 Mejor, dejar al compilador adivinar el tipo. Sabe más que tú:
```csharp
var configTimeMinParam = parameter.Parent.GetParam("CONFIG_MARGIN_TIME") as IntParameter;
```

💡 Mejor, utilizar el método `GetParam<T>` disponible en `Device.cs`:
```csharp
var configTimeMinParam = parameter.Parent.GetParam<IntParameter>("CONFIG_MARGIN_TIME");
```

✅ Solución completa con operadores de nulabilidad
```csharp
var configTimeMinParam = parameter.Parent.GetParam<IntParameter>("CONFIG_MARGIN_TIME");
int configTimeMin = configTimeMinParam?.Value ?? 10;
```

[Ver en SharpLab](https://sharplab.io/#gist:ef0d90f5c4f8701915b012733c1caf84)

### Operadores
| Operador | Nombre | Ejemplo | Descripción |
|----------|--------|---------|-------------|
| `?.`    | 🔹 Operador de acceso condicional nulo | `obj?.Propiedad` | Accede a un miembro solo si el objeto no es `null`. Devuelve `null` si lo es. |
| `??`    | 🔹 Operador de fusión nula | `x ?? y` | Devuelve `x` si no es `null`; si es `null`, devuelve `y`. |
| `!`     | ⚠️ Operador null-forgiving | `string s = posibleNull!;` | Indica al compilador que un valor que podría ser `null` **no lo es**, suprimiendo advertencias. |
| `??=`   | 🔹 Operador de asignación de fusión nula | `x ??= valorPorDefecto;` | Asigna `valorPorDefecto` solo si `x` es `null`. |
