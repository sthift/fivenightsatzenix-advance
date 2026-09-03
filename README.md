# Five Nights at Zenix - Advance

Avance y seguimiento de desarrollo del juego indie de terror **Five Nights at Zenix**, desarrollado en **Godot Engine**.

Desarrollado por **Zenix Games** | [Canal de Zenix Games en YouTube](https://www.youtube.com/@zenixgames-jzf)  
Basado en la serie original de **Zenix Animation**.

🎬 **Canal oficial:** [Zenix Animation en YouTube](https://www.youtube.com/@ZenixAnimation)

---

## 📌 Estado del Proyecto
* [x] Migración del proyecto a Godot Engine.
* [x] Configuración del repositorio y flujo de trabajo.
* [ ] Programación de puertas y luces de la oficina.
* [ ] Inteligencia artificial de los animatrónicos.
* [ ] Sistema de cámaras de seguridad.

---

## 👥 Equipo
* **Dirección / Colaborador Principal:** zenix/sthift
* **Programador Principal:** Josel012

---

## 💻 Ejemplos de Código / Lógica del Juego

### Control de Iluminación y Puertas
> Oficina, Cámara del jugador, Luces, Puertas

```gdscript
extends Node2D
#
var izq:bool = false
var der:bool = false
var puertaIzq:bool = false
var puertaDer:bool = false
var luzIzq:bool = false
var luzDer:bool = false

func _ready() -> void:
	pass



func _process(delta: float) -> void:
	movCamara(izq, der)
	puertas()
	pass

func movCamara(izq:bool, der:bool):
	if izq && $Camara.position.x > -619:
		$Camara.position.x -= 20
	if der && $Camara.position.x < 619:
		$Camara.position.x += 20

func _on_izq_mouse_entered() -> void:
	izq = true
	pass


func _on_izq_mouse_exited() -> void:
	izq = false
	pass


func _on_der_mouse_entered() -> void:
	der = true
	pass


func _on_der_mouse_exited() -> void:
	der = false	
	pass


func _on_boton_izq_puerta_pressed() -> void:
	if not puertaIzq:
		$Auidios/Close_Door.play()
		$animations/Botones/ButtonL.play("puerta")
		$"animations/door left".play("default")
		puertaIzq = true
	else:
		$Auidios/Open_Door.play()
		$animations/Botones/ButtonL.play("apagado")
		$"animations/door left".play_backwards("default")
		puertaIzq = false
	pass


func _on_boton_der_puerta_pressed() -> void:
	if not puertaDer:
		$Auidios/Close_Door.play()
		$animations/Botones/ButtonR.play("puerta")
		$"animations/door right".play("default")
		puertaDer = true
	else:
		$Auidios/Open_Door.play()
		$animations/Botones/ButtonR.play("apagado")
		$"animations/door right".play_backwards("default")
		puertaDer = false
	pass


func _on_boton_izqluz_pressed() -> void:
	if not luzIzq:
		luzIzq = true
		$Auidios/Light_On.play()
	else:
		luzIzq = false
		$animations/Office.play("Office") 
		$animations/Botones/ButtonL.play("apagado")
		$Auidios/Light_On.stop()
	pass


func _on_boton_derluz_pressed() -> void:
	if not luzDer:
		luzDer = true
		$Auidios/Light_On.play()
	else:
		luzDer = false
		$animations/Office.play("Office")
		$animations/Botones/ButtonR.play("apagado")
		$Auidios/Light_On.stop()
	pass

func puertas() -> void:
	if luzIzq and puertaIzq:
		$animations/Botones/ButtonL.play("ambos")
		$animations/Office.play("luzIzq")
		await get_tree().create_timer(.5).timeout
		$animations/Office.stop
	elif luzIzq:
		$animations/Botones/ButtonL.play("luz")
		$animations/Office.play("luzIzq")
		await get_tree().create_timer(.5).timeout
		$animations/Office.stop
	elif puertaIzq:
		$animations/Botones/ButtonL.play("puerta")
		
	if luzDer and puertaDer:
		$animations/Botones/ButtonR.play("ambos")
		$animations/Office.play("luzDer")
		await get_tree().create_timer(.5).timeout
		$animations/Office.stop
	elif luzDer:
		$animations/Botones/ButtonR.play("luz")
		$animations/Office.play("luzDer")
		await get_tree().create_timer(.5).timeout
		$animations/Office.stop
	elif puertaDer:
		$animations/Botones/ButtonR.play("puerta")
