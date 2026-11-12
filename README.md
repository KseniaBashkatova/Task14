## Практическая работа №14
```Csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Practic_14
{
    public readonly struct GeoCoordinate
    {
        public double Latitude { get; }
        public double Longitude { get; }

        public GeoCoordinate(double latitude, double longitude)
        {
            // (если вышли за границы, берем крайнее значение (clamp))
            Latitude = latitude switch
            {
                < -90 => -90,
                > 90 => 90,
                _ => latitude
            };

            Longitude = longitude switch
            {
                < -180 => -180,
                > 180 => 180,
                _ => longitude
            };
        }

        public double DistanceTo(GeoCoordinate other)
        {
            double deltaLat = Latitude - other.Latitude;
            double deltaLon = Longitude - other.Longitude;
            return Math.Sqrt(deltaLat * deltaLat + deltaLon * deltaLon) * 111.0;
        }

        public override string ToString()
        {
            return $"({Latitude:F4}, {Longitude:F4})";
        }
    }

    public class Vector3D
    {
        // поля заменены на свойства для соблюдения инкапсуляции
        public double X { get; set; }
        public double Y { get; set; }
        public double Z { get; set; }

        public Vector3D(double x, double y, double z)
        {
            X = x;
            Y = y;
            Z = z;
        }

        public double Length => Math.Sqrt(X * X + Y * Y + Z * Z);

        // <summary>
        // прибавляет координаты другого вектора к текущему.
        // </summary>
        public void Add(Vector3D other)
        {
            if (other == null) throw new ArgumentNullException(nameof(other));
            X += other.X;
            Y += other.Y;
            Z += other.Z;
        }

        // <summary>
        // возвращает новый вектор, не меняя текущий
        // </summary>
        public Vector3D Added(Vector3D other)
        {
            return new Vector3D(X + other.X, Y + other.Y, Z + other.Z);
        }

        public override string ToString()
        {
            return $"({X:F2}, {Y:F2}, {Z:F2})";
        }
    }

    internal class Program
    {
        static void Main(string[] args)
        {
            // демонстрация работы GeoCoordinate
            var moscow = new GeoCoordinate(55.7558, 37.6173);
            var spb = new GeoCoordinate(59.9343, 30.3351);
            var pole = new GeoCoordinate(100.0, 0.0);

            Console.WriteLine($"Москва: {moscow}");
            Console.WriteLine($"Полюс (с коррекцией): {pole}");
            Console.WriteLine($"Расстояние (упрощенное): {moscow.DistanceTo(spb):F1} км\n");

            // демонстрация работы Vector3D
            var vectorA = new Vector3D(1, 2, 3);
            var vectorB = new Vector3D(4, 5, 6);

            Console.WriteLine($"Вектор A: {vectorA} (Длина: {vectorA.Length:F2})");
            Console.WriteLine($"Вектор B: {vectorB}");

            vectorA.Add(vectorB);
            Console.WriteLine($"A + B (изменение текущего): {vectorA}");

            // проверка неизменности при использовании метода, возвращающего новый объект
            var vectorC = new Vector3D(1, 1, 1);
            var vectorD = vectorC.Added(vectorB);
            Console.WriteLine($"C: {vectorC} (не изменился)");
            Console.WriteLine($"D (новый вектор C + B): {vectorD}");
        }
    }
}
```
### Результат выполнения:
<img width="1435" height="884" alt="1" src="https://github.com/user-attachments/assets/f962f38d-c98f-4606-a762-6db733d45366" />
<img width="1433" height="887" alt="2" src="https://github.com/user-attachments/assets/3d062f55-6374-4507-be93-be9a002ef6e2" />
<img width="1432" height="888" alt="3" src="https://github.com/user-attachments/assets/602a2a35-2eeb-4f90-ac90-261b402dfc48" />
// Делал Аристов Максим
