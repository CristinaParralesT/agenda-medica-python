from optparse import Option
import os
#FUNMCION PARA QUE SE PUEDA LLER EL ARCHIVO DE PACIENTESY DEVOLVER UN DICCIONARIO
def leer_pacientes(archivo):
    pacientes = {}
    if os.path.exists(archivo):
        with open(archivo,'r') as f: 
            for linea in f:
                cedula, nombre = linea.strip().split(',')
                pacientes[cedula] = nombre
    return pacientes
#FUNCION PARA ESCRIBIR UN NUEVO PACIENTE EN EL ARCHIVO
def agregar_paciente(archivo, cedula, nombre):
    with open(archivo,'a') as f:
        f.write(f"{cedula},{nombre}\n")
turnos_por_servicio = {
    'consultas': {},
    'examenes': {},
    'procedimientos': {}
}
todos_los_turnos = {}
numero_turno = 1
def generar_turno (pacientes, cedula, servicio):
    global numero_turno
    if cedula not in pacientes:
        nombre=input ("Ingrese el nombre del paciente:")
        agregar_paciente('pacientes.txt',cedula, nombre)
        pacientes[cedula] = nombre
    turnos_por_servicio[servicio][cedula] = pacientes[cedula]
    todos_los_turnos[numero_turno] = cedula
    numero_turno +=1
def ver_turnos_por_servicio(servicio):
    if servicio in turnos_por_servicio:
        for cedula, nombre in turnos_por_servicio[servicio].items():
            print(f"Cédula: {cedula}, Nombre: {nombre}")
    else:
        print("El servicio no fue encontrado")
def ver_todos_los_turnos():
    for turno, cedula in todos_los_turnos.items():
        print(f"Turno: {turno}, Cédula: {cedula}, Nombre: {pacientes[cedula]}") # type: ignore
# MENÚ PRINCIPAL
def menu():
    pacientes = leer_pacientes('pacientes.txt')
    while True:
        print("\nMenu:")
        print("1. Agregar paciente")
        print("2. Generar turno")
        print("3. Ver turnos por servicio")
        print("4. Ver todos los turnos")
        print("5. Salir")
        opcion = input("Seleccione una opción: ")

        if opcion == '1':
            cedula = input("Ingrese la cédula del paciente: ")
            nombre = input("Ingrese el nombre del paciente: ")
            agregar_paciente('pacientes.txt', cedula, nombre)
            pacientes[cedula] = nombre
        elif opcion == '2':
            cedula = input("Ingrese la cédula del paciente: ")
            servicio = input("Ingrese el servicio (consultas, examenes, procedimientos): ")
            generar_turno(pacientes, cedula, servicio)
        elif opcion == '3':
            servicio = input("Ingrese el servicio (consultas, examenes, procedimientos): ")
            ver_turnos_por_servicio(servicio)
        elif opcion == '4':
            ver_todos_los_turnos()
        elif opcion == '5':
            print("Saliendo del programa...")
            break
        else:
            print("Opción no válida")

# SE EJECUTA EL MENÚ
menu()
