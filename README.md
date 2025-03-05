Bridge
// Define la interfaz para la implementación (Bridge)
public interface IDispositivo
{
    void Encender();
    void Apagar();
    void AjustarVolumen(int nivel);
}
// Implementación concreta: Televisor
public class Televisor : IDispositivo
{
    public void Encender() => Console.WriteLine("TV encendida.");
    public void Apagar() => Console.WriteLine("TV apagada.");
    public void AjustarVolumen(int nivel) => Console.WriteLine($"TV volumen ajustado a {nivel}.");
}

// Implementación concreta: Radio
public class Radio : IDispositivo
{
    public void Encender() => Console.WriteLine("Radio encendida.");
    public void Apagar() => Console.WriteLine("Radio apagada.");
    public void AjustarVolumen(int nivel) => Console.WriteLine($"Radio volumen ajustado a {nivel}.");
}
// Clase abstracta que actúa como Bridge
public abstract class ControlRemoto
{
    protected IDispositivo dispositivo;

    protected ControlRemoto(IDispositivo dispositivo)
    {
        this.dispositivo = dispositivo;
    }

    public virtual void Encender() => dispositivo.Encender();
    public virtual void Apagar() => dispositivo.Apagar();
}
// Control remoto avanzado con funcionalidades extra
public class ControlRemotoAvanzado : ControlRemoto
{
    public ControlRemotoAvanzado(IDispositivo dispositivo) : base(dispositivo) { }

    public void SubirVolumen() => dispositivo.AjustarVolumen(10);
    public void BajarVolumen() => dispositivo.AjustarVolumen(0);
}
class Program
{
    static void Main()
    {
        IDispositivo tv = new Televisor();
        IDispositivo radio = new Radio();

        // Control remoto básico para TV
        ControlRemoto controlTV = new ControlRemotoAvanzado(tv);
        controlTV.Encender();
        ((ControlRemotoAvanzado)controlTV).SubirVolumen();
        controlTV.Apagar();

        Console.WriteLine();

        // Control remoto avanzado para Radio
        ControlRemoto controlRadio = new ControlRemotoAvanzado(radio);
        controlRadio.Encender();
        ((ControlRemotoAvanzado)controlRadio).BajarVolumen();
        controlRadio.Apagar();
    }
}

public interface IEmpleado
{
    void MostrarDetalles();
}
public class Empleado : IEmpleado
{
    private string nombre;
    private string puesto;

    public Empleado(string nombre, string puesto)
    {
        this.nombre = nombre;
        this.puesto = puesto;
    }

    public void MostrarDetalles()
    {
        Console.WriteLine($"{nombre} - {puesto}");
    }
}
using System;
using System.Collections.Generic;

public class Gerente : IEmpleado
{
    private string nombre;
    private string puesto;
    private List<IEmpleado> subordinados = new List<IEmpleado>();

    public Gerente(string nombre, string puesto)
    {
        this.nombre = nombre;
        this.puesto = puesto;
    }

    public void AgregarSubordinado(IEmpleado empleado)
    {
        subordinados.Add(empleado);
    }

    public void EliminarSubordinado(IEmpleado empleado)
    {
        subordinados.Remove(empleado);
    }

    public void MostrarDetalles()
    {
        Console.WriteLine($"{nombre} - {puesto} (Gerente)");
        Console.WriteLine("Subordinados:");
        foreach (var empleado in subordinados)
        {
            empleado.MostrarDetalles();
        }
    }
}
class Program
{
    static void Main(string[] args)
    {
        // Crear empleados individuales
        IEmpleado emp1 = new Empleado("Juan Pérez", "Desarrollador");
        IEmpleado emp2 = new Empleado("Ana Gómez", "Diseñadora");

        // Crear gerente y asignar subordinados
        Gerente gerente = new Gerente("Carlos López", "Jefe de Proyecto");
        gerente.AgregarSubordinado(emp1);
        gerente.AgregarSubordinado(emp2);

        // Crear otro gerente
        Gerente director = new Gerente("María Rodríguez", "Directora de Tecnología");
        director.AgregarSubordinado(gerente);

        // Mostrar estructura organizativa
        director.MostrarDetalles();
    }
}
María Rodríguez - Directora de Tecnología (Gerente)
Subordinados:
Carlos López - Jefe de Proyecto (Gerente)
Subordinados:
Juan Pérez - Desarrollador
Ana Gómez - Diseñadora
