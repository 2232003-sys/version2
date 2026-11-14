# Sr. Diente — especificación funcional para Stitch

Fecha: 13 de noviembre de 2026. Fuente: revisión estática del proyecto local. No constituye una prueba de producción ni aprobación visual de pantallas.

## Instrucción para Stitch
Rediseña el sistema existente siguiendo esta especificación. No tienes acceso al repositorio: este documento es tu referencia. Diferencia E (capacidad observada en código), D (diseño propuesto sobre esa capacidad) y N (función nueva que requiere desarrollo/decisión). Un modelo de datos no garantiza que exista una acción para modificar todos sus campos. No inventes reglas, permisos, estados ni integraciones. Las preferencias propuestas no equivalen a decisiones aprobadas por el usuario.
Mantén matriz por pantalla: pendiente de diseñar / diseñada pendiente de revisión / aprobada explícitamente por usuario. Incluye enlace o nombre de pantalla. No apruebes tus propios resultados.

## Diseño común (D)
Español de México, MXN, zona America/Mexico_City. Blanco, azul #0284C7, turquesa #2DD4BF, grises claros; Outfit y Manrope como referencia. Escritorio 1440/1280, tablet 768, móvil 390. Navegación consistente, tablas legibles, formularios agrupados, controles táctiles >=44 px. Cada vista: carga, vacío, error/reintento, éxito, permisos, validación y cambios sin guardar. Los errores conservan captura. Conflictos de actualización ofrecen recargar sin sobrescribir silenciosamente. Notas técnicas y simuladores fuera del producto.

## Navegación actual (E)
Dashboard, Agenda, Pacientes e Inventario aparecen para todos los roles. Doctores y Servicios: dueño/administrador. Comisiones: dueño/doctor. Caja: dueño/administrador/recepción. Finanzas: dueño/administrador. Presupuestos y Club Sonrisas: dueño/administrador/recepción. Usuarios: dueño. Esto describe el menú, no todos los permisos de la API. No ampliar permisos por conveniencia visual.

## 1. Acceso y usuarios
E: login por correo y contraseña, usuarios con nombre, correo, rol, activo y doctor asociado. Roles: dueño, administrador, recepción y doctor. El dueño lista, crea, edita y elimina cuentas. No puede eliminarse a sí mismo. Cambio de contraseña: dueño sobre cualquier usuario; cada usuario sobre sí mismo. Mínimo actual en ese endpoint: 6 caracteres. No hay sede en el formulario de acceso.
D: login, error, sesión vencida y permiso denegado; directorio, alta, edición, activar/desactivar, confirmar eliminación y cambio de contraseña. Alta: nombre, correo, contraseña, rol y doctor asociado cuando corresponda. Edición no debe prometer cambio de correo: el modelo de actualización no lo incluye. Confirmación de contraseña es validación de interfaz propuesta. Mostrar permisos por rol sin inventar permisos configurables.
N: recuperación por correo, PIN, desbloqueo rápido, suspensión como tercer estado, selección de sede.

## 2. Dashboard
E: resumen, citas del día y reportes por periodo. Modelo de resumen: citas hoy, próximas 7 días, ingresos hoy/mes, pacientes nuevos del mes, doctores activos y pacientes totales.
D: vista de recepción con citas, confirmaciones y pendientes; doctor con sus pacientes/citas; dueño con indicadores disponibles. Cada KPI indica periodo y abre detalle. Separar cobrado de presupuestado. Métricas nuevas deben etiquetarse como N hasta definir cálculo. No prometer actualización en vivo.
Pantallas: resumen general, variantes por rol, filtros de reporte, detalle de indicador, vacío/error.

## 3. Agenda y atención
E: paciente, doctor, servicio opcional, presupuesto relacionado opcional, fecha/hora, duración, sillón, notas, precio de reserva, laboratorio y campos financieros legados. Sincronización Google: pendiente/sincronizado/error. Estados del modelo: agendada, confirmada, en atención, completada, cancelada por paciente/doctor/clínica, no asistió y reprogramada; existen estados legados de cancelación. “Llegó” no está en esa enumeración y es N.
E: nota de evolución y confirmaciones clínicas. No confundir borrador de nota con borrador de cita.
D: calendario diario/semanal, filtros doctor/sillón/estado, nueva cita, detalle, edición, reprogramación, cancelación con motivo, historial y atención clínica. Móvil lista del día; tablet tres columnas cuando sean legibles. Doctor y sillón independientes, sujetos a disponibilidad y restricciones. Elegir paciente existente antes de crear. Mantener captura al alta contextual. “Horario seleccionado”, nunca reservado. Disponibilidad consultada se valida al guardar. Fallo de guardado no libera ni confirma recursos. Importes cobrados deben enlazar con Caja: reorganizar captura financiera legada requiere revisión de implementación.
Pantallas adicionales: nota clínica (diagnóstico, tratamiento, materiales), confirmaciones según permisos, conflicto de horario y error de Calendar. Roles clínicos y financieros no deben compartir todas las acciones.

## 4. Pacientes
E: nombre; nacimiento, género, teléfono, correo, dirección, contacto de emergencia/número, alergias, padecimientos, medicamentos, notas, URL de foto, doctor principal y fuente de adquisición. En el modelo sólo nombre es obligatorio: hacer celular obligatorio sería un cambio, no regla actual.
E: alertas estructuradas de enfermedades, alergias y problemas cardiacos; medicamentos, embarazo, observaciones; paciente especial, instrucciones y roles de visibilidad. Riesgo bajo/medio/alto/crítico registrado, no inferido por diseño.
E: perfil de pago con confianza manual, límite de crédito, permite plan, requiere anticipo, porcentaje y observaciones administrativas.
E: lista de comprobación: consentimiento firmado, identificación recibida, estudios recibidos, presupuesto firmado, convenio firmado, documentos pendientes y observaciones. No es un repositorio de archivos.
E: seguimientos por WhatsApp/llamada; resultado sin respuesta/contactado/quiere cita/cita agendada/no interesado; notas, próxima fecha y cita relacionada. Se registra autor y fecha. Crear/editar seguimiento: dueño/admin/recepción. Doctor tiene alcance limitado por relaciones del paciente.
D: directorio, filtros 3/6/12 meses sin visita, sin visitas y seguimiento pendiente; alta/edición; ficha con resumen, citas, antecedentes/alertas, tratamientos, pagos, seguimientos, documentos y membresía. Saldos global y por tratamiento separados. Detalle de seguimiento y formulario con próxima acción. Posible duplicado es advertencia, no fusión automática.
N: odontograma, repositorio radiográfico, subir/descargar archivos y fusión automática. Detección predictiva de duplicados debe validarse técnicamente antes de prometerla.

## 5. Doctores
E: nombre, especialidad, teléfono, correo, tipo general/especialista/administrativo, porcentaje de comisión, color, Calendar ID y activo. No hay cédula profesional ni horario en DoctorIn.
D: directorio, alta/edición, perfil, estado de Calendar y sincronización manual; distinguir doctor de cuenta de usuario. Mostrar disponibilidad sólo según configuración existente verificada; un editor de horario propio nuevo es N. Desactivar conservando historial es recomendación D; no afirmar que toda eliminación actual ya funciona así.
N: cédula profesional y nuevas reglas de comisión por procedimiento si no corresponden a reglas existentes. No vincular sillón fijo a doctor.

## 6. Servicios
E: nombre, descripción, precio, duración en minutos, categoría y activo; listar, crear, actualizar y eliminar. Consulta autenticada, mutaciones dueño/admin.
D: catálogo, búsqueda/categoría, alta/edición, detalle y confirmación de retiro. Mostrar tarifas Club en contexto sin duplicar servicio. Categoría existe como campo: una administración independiente de categorías sería N. Guardar precio histórico del presupuesto según su captura, sin recalcular por un cambio de catálogo.

## 7. Presupuestos y planes
E: paciente, responsable, vigencia, diagnóstico, notas, subtotal, descuentos, total, anticipo y financiado. Conceptos con servicio, fase, pieza/área, descripción, cantidad, precio, descuento, total, doctor, estado, notas y laboratorio. Contexto Club conserva datos históricos de tarifa y regla. Colaboraciones commission_shares y transferencia de doctor están contempladas.
E: estados de presupuesto: borrador, presentado, aceptado, rechazado, aceptación parcial, en tratamiento, completado, cancelado y vencido. No reducirlos a cinco estados sin mapear sus agrupaciones. Conceptos: pendiente/autorizado/programado/en proceso/completado/cancelado.
E: planes con responsable de pago y teléfono, total, anticipo, financiado, frecuencia única/mensual/quincenal/semanal/personalizada, cantidad, inicio y cuotas. Cuotas pendiente/parcial/pagada/vencida/cancelada. Firma de paciente y clínica en convenio; no afirmar biometría ni validez legal certificada.
D: listado con agrupaciones explícitas, creación por pasos, edición, presentación, aceptación/rechazo, detalle de conceptos, calendario de cuotas, impresión presupuesto/convenio, cierre y abono en Caja. Separar ejecución clínica de pago; mostrar procedimientos completados, no porcentaje inventado. Recepción tiene límite documentado de descuento del 10% por concepto en edición; mostrar error del servidor, no bypass.
N: editor de adendas y comparador de versiones contractuales. Versión técnica para conflictos no equivale a gestión contractual de versiones.

## 8. Caja y recibos
E: apertura con fondo y notas; cierre con efectivo contado, conteos por método y notas. Pago relacionado con paciente y opcionalmente cita/doctor/presupuesto/plan. Conceptos con descripción, cantidad, precio, descuento y total. El modelo acepta varios métodos con tipo e importe; referencia, banco, últimos 4 dígitos y autorización opcionales. Pago mixto está contemplado en el contrato de datos, aunque falta verificar recorrido completo en ejecución.
E: movimientos con tipo, método, importe, concepto, fecha de negocio, beneficiario y notas. Reversión de cobro dueño/admin. Apertura/cierre/cobro: dueño/admin/recepción.
D: caja cerrada/apertura, resumen abierta, pendientes, cobrar, distribuir abono, movimientos, deudores, historial, recibo imprimible, reversión y cierre. Cobrar: paciente → tratamiento/conceptos → importe/métodos → revisión → guardar → recibo/saldos. Suma de métodos coincide con recibido. No duplicar cobro al reintentar. Mensajes de error no deben prometer idempotencia universal sin verificar operación.
D: separar saldo del tratamiento y global; movimientos en efectivo afectan arqueo, tarjeta/transferencia se muestran aparte. Arqueo esperado/contado/diferencia, no ciego. “Depósito a Arnold” existe en pantalla: su significado contable necesita decisión del usuario antes de renombrarlo.
N: CFDI, cobro bancario automático, terminal integrada. Registrar tarjeta no procesa una tarjeta.

## 9. Finanzas
E: pantalla de movimientos manuales por mes con fecha, dirección ingreso/egreso, categoría, concepto, método, monto y notas. Totales ingresos/egresos/balance; crear/eliminar dueño/admin. No modifica Caja ni crea pagos de pacientes. Existen historial de cierres mensuales e importación en backend, distintos de la pantalla manual.
D: selector de mes, tabla, formulario, confirmación de eliminación y origen de cifras. Histórico importado requiere vista específica propuesta, no afirmar que la actual lo contiene.
N: consolidación automática, conciliación bancaria y utilidad contable. No sumar registros manuales y operativos sin regla contra duplicación.

## 10. Comisiones
E: resumen general dueño; detalle dueño o doctor (doctor sólo propio). Reglas generales dueño. Consulta costo de tarjeta dueño/admin, edición dueño. Registro de pago dueño/admin y acción de pago de payout dueño según endpoints.
E: política: primero laboratorio del tratamiento, luego clínico; comisión sobre clínico cobrado, tasa congelada. Devengado, pagado y pendiente separados. Colaboraciones por participación; costo de tarjeta repartido según política. Históricos sin trazabilidad detallada conservan cálculo legado.
D: resumen por periodo/doctor, detalle por cobro, desglose laboratorio/clínico/tasa/bruto/costo tarjeta/neto, formulario de pago e historial. No presentar registro interno como transferencia bancaria. No mezclar doctores de tratamientos independientes.

## 11. Inventario
E: tipo material/utensilio, clave numérica opcional, nombre, unidad, entrada, salida, stock, mínimo, observaciones, caducidad y activo. Ajuste con cantidad positiva/negativa, motivo y notas. Transacción registra anterior, nuevo, autor y fecha. Las rutas observadas usan autenticación general, no una separación estricta por rol.
D: listado con tipo/búsqueda, alta/edición, detalle, entrada/salida/ajuste, historial, stock bajo y caducidad. No presentar stock como campo libre cuando se diseña un ajuste: guardar mediante movimiento trazable es recomendación D. Restringir ajustes/eliminación a roles concretos sería cambio de permisos N.
N: lotes, órdenes de compra, proveedores de insumos, consumo automático por tratamiento.

## 12. Club Sonrisas
E: seis secciones Resumen, Empresas, Convenios, Afiliados, Tarifas y Analítica.
Empresas: nombre comercial obligatorio, razón social, RFC, domicilio fiscal, contacto/cargo/correo/teléfono, número de empleados, estado y notas. Estados prospecto/activa/suspendida/vencida/cancelada. Crear/editar dueño/admin; eliminar dueño; consultar dueño/admin/recepción.
Convenios: empresa, nombre, inicio/fin opcional, estado, máximo beneficiarios (valor inicial del modelo 5, configurable, no regla universal), parentescos permitidos, pagador paciente/empresa, notas y términos. Folio generado CS-YYYY-0001. Estados borrador/activo/suspendido/vencido/cancelado. Crear/editar/eliminar dueño; consultar dueño/admin/recepción.
Afiliados: empresa, convenio, empleado o beneficiario, número empleado, nombres/apellidos, teléfono/correo/nacimiento, parentesco, titular cuando beneficiario, paciente vinculado, estado, alta/baja y notas. Estados activo/suspendido/inactivo. Parentescos cónyuge/pareja/hijo/padre/madre/hermano/otro. Crear/editar dueño/admin; vincular paciente también recepción; doctor consulta sólo dentro de su alcance.
Tarifas: servicio, convenio o empresa o alcance global; precio fijo/descuento porcentual/descuento de importe/gratis; valor, prioridad, empleado/beneficiario, máximo usos opcional, combinable con promociones, activa, vigencia y notas. Crear/editar/eliminar dueño/admin; consultar también recepción. Resolver precio: regla convenio → empresa → global Club → catálogo público. No escoger automáticamente el mayor descuento saltándose esta jerarquía.
Beneficios: consultar elegibilidad y precio por afiliado/paciente y servicio; mostrar base, final, ahorro y motivo de no elegibilidad. Registro de uso relaciona afiliado, regla y servicio con presupuesto/pago cuando exista; estados reservado/consumido/cancelado. Consultar precio no equivale a consumir beneficio.
Analítica: dueño/admin; empleados activos/inactivos, con/sin uso, tasa de utilización, por empresa y evolución mensual. Uso significa beneficio consumido dentro del periodo; presupuesto creado no cuenta como uso. No llamar rentabilidad a esta analítica ni afirmar que mide todos los beneficiarios: el cálculo observado parte de empleados.
D pantallas: resumen; directorio/alta/edición/detalle empresa; listado/alta/edición/detalle convenio; padrón/alta/edición/detalle afiliado; beneficiarios del titular; vinculación paciente; listado/alta/edición/detalle tarifa; validación; historial de usos; analítica. Mostrar campos condicionales según tipo de afiliado/regla. Validaciones autoritativas del backend conservan captura.

## 13. Laboratorio
E backend, sin pestaña propia actual: crear orden dueño/admin/recepción; consultar orden autenticado; pagar proveedor dueño/admin. Orden: paciente, presupuesto/plan/cita opcionales, identificador proveedor opcional, descripción, costo pactado, anticipo requerido/pagado. Valida coherencia paciente-plan-presupuesto. Si anticipo insuficiente, se bloquea salvo autorización dueño/admin con motivo. Creación produce enviada o autorizada con advertencia.
E: pago proveedor con monto positivo y método, requiere Caja abierta; genera salida y reduce efectivo esperado si corresponde. Orden cancelada no admite pago. No confundir anticipo del paciente con pago al proveedor.
D: crear orden desde tratamiento, detalle, advertencia/solicitud de autorización y pago proveedor. Listado global e historial visible de pagos necesitan soporte adicional: el router observado sólo expone crear, obtener orden y pagar. El modelo enumera borrador/enviada/en proceso/recibida/completada/cancelada, pero no hay endpoint de actualización de estado en ese router. Mostrar estado es E; cambiarlo es N.
N: padrón/tarifarios, logística, facturas, edición de estados y rutas adicionales de consulta. No inventar pago bancario real ni afirmar bloqueo por sobrepago no observado.

## 14. Sillones, cierres y mantenimiento
E backend: sillón con nombre, número, activo, horarios y servicios permitidos. Cierre con fecha, inicio/fin o todo el día, zona, todos o sillones seleccionados, motivo y activo. Mantenimiento con sillón, periodo, motivo, estado activo/cancelado/completado. Tres sillones como referencia actual, sin especialidades fijas.
D: sección de configuración vinculada a Agenda: lista/edición sillón, calendario/formulario de cierre, mantenimiento y conflictos. Nueva ubicación de menú es propuesta D. Permisos exactos se conservan según router de chairs; no asumir edición por recepción.
N: sedes, equipos (compresor/lámpara), inventario de infraestructura y tiempos automáticos de desinfección.

## Fuera del alcance actual
Sitio público permanece separado del panel: landing, tratamientos, ubicación, QR, Club público y legales. Reportes independientes, automatizaciones WhatsApp/SMS, multi-clínica, odontograma y repositorio clínico requieren definición/desarrollo. Enlaces WhatsApp no equivalen a mensajería automática.

## Prototipos obligatorios (D)
1. Buscar paciente → alta si hace falta → nueva cita → conflicto/reintento → agenda.
2. Cita → atención/nota → Caja → pago → recibo → saldos.
3. Paciente → presupuesto → aceptación → cuotas → abono parcial → saldo.
4. Seguimiento → próxima acción → cita relacionada.
5. Comisión → cálculo → registro de pago.
6. Afiliado → elegibilidad → tarifa → presupuesto → uso consumido.
7. Orden laboratorio → anticipo insuficiente → autorización → pago proveedor con Caja abierta.
8. Apertura → movimientos/cobros → cierre con diferencia.

## Criterios de entrega
Cada formulario debe tener campos exactos, obligatoriedad según contrato, validación, acción de guardar, éxito y error. Cada operación sensible, confirmación y rol. Propuestas nuevas se anotan externamente. No inventar métricas ni condiciones para llenar espacio. Nombres y folios deben ser consistentes; usar datos ficticios, sin datos reales de pacientes. No generar otro resumen del plan: producir pantallas por bloques y mantener matriz actualizada.

## Decisiones que pertenecen al usuario
Aprobar visuales; reglas nuevas de llegada; obligatoriedad de teléfono y prevención de duplicados; restricciones nuevas de inventario; consolidación financiera; significado de depósitos; contrato de versiones; ubicación de laboratorio; alcance de nuevos documentos clínicos. No convertir estas propuestas en preferencias confirmadas.

## Evidencia
Fuentes: frontend/src/admin/AdminLayout.jsx y pages; backend/models.py; backend/routers; FINANCIAL_POLICY.md; PUNTO_DE_REFERENCIA.md. El anexo siguiente conserva contratos de datos y rutas para consulta técnica; no se muestran estos nombres internos al usuario del producto.

## Anexo de contratos verificados en modelos

Los campos sin valor predeterminado son requeridos por ese modelo. Un campo opcional en datos no implica que sea editable en interfaz.

### BaseDocument
```python
class BaseDocument(BaseModel):
    """Base for all stored documents. Uses UUID string IDs (not ObjectId)."""

    model_config = ConfigDict(populate_by_name=True, extra="ignore")

    id: str = Field(default_factory=new_id)
    created_at: datetime = Field(default_factory=utcnow)
    updated_at: datetime = Field(default_factory=utcnow)

    def to_mongo(self) -> dict[str, Any]:
        doc = self.model_dump()
        # Store datetimes as ISO strings to avoid driver-specific issues.
        for k, v in list(doc.items()):
            if isinstance(v, datetime):
                doc[k] = v.isoformat()
        return doc

    @classmethod
    def from_mongo(cls, doc: dict[str, Any] | None):
        if doc is None:
            return None
        data = dict(doc)
        data.pop("_id", None)
        # Convert ISO strings back to datetime if needed
        for f in ("created_at", "updated_at"):
            if f in data and isinstance(data[f], str):
                try:
                    data[f] = datetime.fromisoformat(data[f])
                except ValueError:
                    pass
        return cls(**data)
```

### UserRole
```python
class UserRole:
    OWNER = "owner"
    ADMIN = "admin"
    RECEPTION = "reception"
    DOCTOR = "doctor"
```

### User
```python
class User(BaseDocument):
    email: str
    password_hash: str
    name: str
    role: str = UserRole.RECEPTION
    is_active: bool = True
    doctor_id: Optional[str] = None
```

### UserPublic
```python
class UserPublic(BaseModel):
    id: str
    email: str
    name: str
    role: str
    is_active: bool
    doctor_id: Optional[str] = None
    created_at: datetime
```

### LoginIn
```python
class LoginIn(BaseModel):
    email: EmailStr
    password: str
```

### UserCreateIn
```python
class UserCreateIn(BaseModel):
    email: EmailStr
    password: str
    name: str
    role: str
    doctor_id: Optional[str] = None
```

### UserUpdateIn
```python
class UserUpdateIn(BaseModel):
    name: Optional[str] = None
    role: Optional[str] = None
    is_active: Optional[bool] = None
    doctor_id: Optional[str] = None
```

### PasswordChangeIn
```python
class PasswordChangeIn(BaseModel):
    new_password: str
```

### Doctor
```python
class Doctor(BaseDocument):
    name: str
    specialty: Optional[str] = None
    phone: Optional[str] = None
    email: Optional[str] = None
    doctor_type: str = "general"  # "general" | "specialist" | "administrative"
    commission_percent: float = 0.0  # % of service price
    color: str = "#0284c7"  # for calendar
    google_calendar_id: Optional[str] = None
    is_active: bool = True
```

### DoctorIn
```python
class DoctorIn(BaseModel):
    name: str
    specialty: Optional[str] = None
    phone: Optional[str] = None
    email: Optional[str] = None
    doctor_type: str = "general"
    commission_percent: float = 0.0
    color: str = "#0284c7"
    google_calendar_id: Optional[str] = None
    is_active: bool = True
```

### Service
```python
class Service(BaseDocument):
    name: str
    description: Optional[str] = None
    price: float
    duration_minutes: int = 30
    category: Optional[str] = None
    is_active: bool = True
```

### ServiceIn
```python
class ServiceIn(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    duration_minutes: int = 30
    category: Optional[str] = None
    is_active: bool = True
```

### AppointmentStatus
```python
class AppointmentStatus:
    SCHEDULED = "scheduled"
    CONFIRMED = "confirmed"
    IN_PROGRESS = "in_progress"
    COMPLETED = "completed"
    CANCELLED_BY_PATIENT = "cancelled_by_patient"
    CANCELLED_BY_DOCTOR = "cancelled_by_doctor"
    CANCELLED_BY_CLINIC = "cancelled_by_clinic"
    NO_SHOW = "no_show"
    RESCHEDULED = "rescheduled"
    CANCELLED_LEGACY = "cancelled_legacy"
    CANCELLED = "cancelled"
```

### GoogleSyncStatus
```python
class GoogleSyncStatus:
    SYNCED = "synced"
    PENDING = "pending"
    ERROR = "error"
```

### EvolutionNoteStatus
```python
class EvolutionNoteStatus:
    PENDING = "pending"
    DRAFTED = "drafted"
    DOCTOR_CONFIRMED = "doctor_confirmed"
    PATIENT_CONFIRMED = "patient_confirmed"
    COMPLETE = "complete"
```

### TrustLevel
```python
class TrustLevel:
    NO_HISTORY = "no_history"
    RELIABLE = "reliable"
    FOLLOW_UP_REQUIRED = "follow_up_required"
    PAYMENT_RISK = "payment_risk"
```

### RiskLevel
```python
class RiskLevel:
    LOW = "low"
    MEDIUM = "medium"
    HIGH = "high"
    CRITICAL = "critical"
```

### AlertCondition
```python
class AlertCondition(BaseModel):
    status: bool = False
    description: Optional[str] = None
    risk_level: str = RiskLevel.LOW
    registered_at: Optional[datetime] = None
    registered_by: Optional[str] = None
```

### ClinicalAlerts
```python
class ClinicalAlerts(BaseModel):
    enfermedades_sistemicas: dict[str, AlertCondition] = Field(default_factory=dict)
    alergias: dict[str, AlertCondition] = Field(default_factory=dict)
    problemas_cardiacos: dict[str, AlertCondition] = Field(default_factory=dict)
    medicamentos_actuales: Optional[str] = None
    embarazo: bool = False
    observaciones_medicas: Optional[str] = None
    fecha_ultima_actualizacion_medica: Optional[datetime] = None
    actualizado_por: Optional[str] = None
    
    # Paciente especial
    paciente_especial: bool = False
    tipo_alerta: List[str] = Field(default_factory=list)
    instrucciones_de_atencion: Optional[str] = None
    alerta_visible_para: List[str] = Field(default_factory=list)
```

### PaymentProfile
```python
class PaymentProfile(BaseModel):
    nivel_confianza_manual: Optional[str] = None
    trust_level: Optional[str] = None  # "alto" | "medio" | "bajo"
    limite_credito: float = 0.0
    permite_plan_de_pago: bool = True
    requiere_anticipo: bool = False
    porcentaje_anticipo: float = 0.0
    observaciones_administrativas: Optional[str] = None
    nivel_confianza_log: List[dict] = Field(default_factory=list)
```

### DocumentChecklist
```python
class DocumentChecklist(BaseModel):
    consentimiento_firmado: bool = False
    identificacion_recibida: bool = False
    estudios_recibidos: bool = False
    presupuesto_firmado: bool = False
    convenio_firmado: bool = False
    documentos_pendientes: List[str] = Field(default_factory=list)
    observaciones: Optional[str] = None
    revisado_por: Optional[str] = None
    revisado_en: Optional[datetime] = None
```

### AuditLog
```python
class AuditLog(BaseDocument):
    request_id: Optional[str] = None
    user_id: str
    user_name: str
    user_email: str
    actor_role: str
    resource_type: str
    resource_id: str
    action: str
    metadata: dict = Field(default_factory=dict)
    created_at: datetime = Field(default_factory=utcnow)
```

### DentalChair
```python
class DentalChair(BaseDocument):
    name: str
    number: int
    is_active: bool = True
    hours_available: dict = Field(default_factory=dict)
    services_permitted: List[str] = Field(default_factory=list)
    version: int = 1
```

### ClinicClosure
```python
class ClinicClosure(BaseDocument):
    local_date: str  # YYYY-MM-DD
    start_time: Optional[str] = None  # HH:MM
    end_time: Optional[str] = None  # HH:MM
    all_day: bool = False
    timezone: str = "America/Mexico_City"
    starts_at_utc: datetime
    ends_at_utc: datetime
    applies_to_all: bool = True
    chair_ids: Optional[List[str]] = None
    reason: str
    is_active: bool = True
    created_by: str
    version: int = 1
```

### ChairMaintenance
```python
class ChairMaintenance(BaseDocument):
    chair_id: str
    starts_at_utc: datetime
    ends_at_utc: datetime
    local_start: str  # YYYY-MM-DDTHH:MM
    local_end: str  # YYYY-MM-DDTHH:MM
    timezone: str = "America/Mexico_City"
    reason: str
    status: str = "active"  # "active" | "cancelled" | "completed"
    created_by: str
    version: int = 1
```

### ChairSlot
```python
class ChairSlot(BaseModel):
    id: str = Field(default_factory=new_id)
    chair_id: str
    slot_start: datetime  # UTC aware
    appointment_id: str
    reservation_id: str
    status: str = "confirmed"  # "confirmed" | "hold"
    created_at: datetime = Field(default_factory=utcnow)
    expires_at: Optional[datetime] = None
```

### PaymentAllocation
```python
class PaymentAllocation(BaseDocument):
    payment_id: str
    patient_id: Optional[str] = None
    allocation_type: Optional[str] = None  # "estimate" | "payment_plan"
    estimate_id: Optional[str] = None
    payment_plan_id: Optional[str] = None
    installment_id: Optional[str] = None
    treatment_id: Optional[str] = None
    appointment_id: Optional[str] = None
    doctor_id: Optional[str] = None
    amount: float = 0.0
    component_type: Optional[str] = None  # "laboratory" | "clinical"
    laboratory_amount: float = 0.0
    clinical_amount: float = 0.0
    commission_rate: Optional[float] = None
    commission_earned: float = 0.0
    commission_gross: float = 0.0
    commission_net: float = 0.0
    card_processing_fee: float = 0.0
    commission_shares: list[dict] = []
    commission_paid: float = 0.0
    policy_version: Optional[str] = None
    status: str = "active"  # "active" | "reversed"
    created_at: datetime = Field(default_factory=utcnow)
    created_by: Optional[str] = None
    reversed_at: Optional[datetime] = None
    reversed_by: Optional[str] = None
    reversal_reason: Optional[str] = None
    allocation_batch_key: Optional[str] = None
```

### PaymentAllocationRequestItem
```python
class PaymentAllocationRequestItem(BaseModel):
    payment_plan_id: Optional[str] = None
    estimate_id: Optional[str] = None
    amount: float
```

### PaymentAllocationRequest
```python
class PaymentAllocationRequest(BaseModel):
    allocations: list[PaymentAllocationRequestItem]
    idempotency_key: Optional[str] = None
```

### Patient
```python
class Patient(BaseDocument):
    name: str
    dob: Optional[str] = None  # YYYY-MM-DD
    gender: Optional[str] = None  # M/F/O
    phone: Optional[str] = None
    email: Optional[str] = None
    address: Optional[str] = None
    emergency_contact_name: Optional[str] = None
    emergency_contact_phone: Optional[str] = None
    allergies: Optional[str] = None
    medical_conditions: Optional[str] = None
    medications: Optional[str] = None
    notes: Optional[str] = None
    photo_url: Optional[str] = None
    primary_doctor_id: Optional[str] = None
    created_by: Optional[str] = None
    
    # Fase 5 additions
    fuente_adquisicion: Optional[str] = None
    clinical_alerts: Optional[ClinicalAlerts] = None
    payment_profile: Optional[PaymentProfile] = None
    document_checklist: Optional[DocumentChecklist] = None
    version: int = 1
```

### PatientIn
```python
class PatientIn(BaseModel):
    name: str
    dob: Optional[str] = None
    gender: Optional[str] = None
    phone: Optional[str] = None
    email: Optional[str] = None
    address: Optional[str] = None
    emergency_contact_name: Optional[str] = None
    emergency_contact_phone: Optional[str] = None
    allergies: Optional[str] = None
    medical_conditions: Optional[str] = None
    medications: Optional[str] = None
    notes: Optional[str] = None
    photo_url: Optional[str] = None
    primary_doctor_id: Optional[str] = None
    
    # Fase 5 additions
    fuente_adquisicion: Optional[str] = None
    clinical_alerts: Optional[ClinicalAlerts] = None
    payment_profile: Optional[PaymentProfile] = None
    document_checklist: Optional[DocumentChecklist] = None
```

### NoteConfirmationDoctor
```python
class NoteConfirmationDoctor(BaseModel):
    user_id: str
    doctor_id: str
    nombre: str
    confirmed_at: datetime
    acceptance_text: str
    note_version: int
    invalidated_at: Optional[datetime] = None
    invalidation_reason: Optional[str] = None
```

### NoteConfirmationPatient
```python
class NoteConfirmationPatient(BaseModel):
    patient_id: str
    nombre: str
    confirmed_at: datetime
    acceptance_text: str
    recorded_by_user_id: str
    recorded_by_role: str
    note_version: int
    invalidated_at: Optional[datetime] = None
    invalidation_reason: Optional[str] = None
```

### EvolutionNote
```python
class EvolutionNote(BaseModel):
    diagnostico: Optional[str] = None
    tratamiento_realizado: Optional[str] = None
    materiales_utilizados: Optional[str] = None
    medicamentos_recetados: Optional[str] = None
    indicaciones: Optional[str] = None
    proximo_paso: Optional[str] = None
    confirmacion_doctor: Optional[NoteConfirmationDoctor] = None
    confirmacion_paciente: Optional[NoteConfirmationPatient] = None
    confirmaciones_historicas: List[dict] = Field(default_factory=list)
    estado: str = EvolutionNoteStatus.PENDING
    pago_asociado_id: Optional[str] = None
    saldo_posterior: float = 0.0
    cierre_excepcional: bool = False
    cierre_excepcional_motivo: Optional[str] = None
    cierre_excepcional_usuario: Optional[str] = None
    cierre_excepcional_fecha: Optional[datetime] = None
    note_version: int = 1
```

### Appointment
```python
class Appointment(BaseDocument):
    patient_id: str
    doctor_id: str
    estimate_id: Optional[str] = None
    service_id: Optional[str] = None
    scheduled_at: str  # ISO string for compatibility
    duration_minutes: int = 30
    status: str = AppointmentStatus.SCHEDULED
    notes: Optional[str] = None
    price_at_booking: Optional[float] = None
    amount_paid: float = 0.0
    payment_method: Optional[str] = None
    laboratory_cost: float = 0.0
    financial_tracking_mode: Optional[str] = None
    laboratory_paid: float = 0.0
    clinical_paid: float = 0.0
    commission_earned: float = 0.0
    commission_paid: float = 0.0
    commission_shares: list[dict] = []
    created_by: Optional[str] = None
    google_event_id: Optional[str] = None
    google_calendar_id: Optional[str] = None
    google_sync_status: str = GoogleSyncStatus.PENDING
    google_last_synced_at: Optional[str] = None
    google_sync_error: Optional[str] = None
    source: str = "system"
    
    # Fase 5 additions
    scheduled_start: Optional[datetime] = None
    scheduled_end: Optional[datetime] = None
    chair_id: Optional[str] = None
    cancelado_por: Optional[str] = None
    cancelacion_motivo: Optional[str] = None
    cancelacion_fecha: Optional[datetime] = None
    cancelacion_usuario_id: Optional[str] = None
    horas_de_anticipacion: Optional[float] = None
    reprogramada: bool = False
    nueva_cita_relacionada_id: Optional[str] = None
    status_history: List[dict] = Field(default_factory=list)
    actual_start_time: Optional[datetime] = None
    actual_end_time: Optional[datetime] = None
    evolution_note: Optional[EvolutionNote] = None
    version: int = 1
```

### AppointmentIn
```python
class AppointmentIn(BaseModel):
    patient_id: str
    doctor_id: str
    estimate_id: Optional[str] = None
    service_id: Optional[str] = None
    scheduled_at: str
    duration_minutes: int = 30
    status: str = AppointmentStatus.SCHEDULED
    notes: Optional[str] = None
    price_at_booking: Optional[float] = None
    amount_paid: float = 0.0
    payment_method: Optional[str] = None
    laboratory_cost: float = 0.0
    financial_tracking_mode: Optional[str] = None
    commission_shares: list[dict] = []
    
    # Fase 5 additions
    scheduled_start: Optional[datetime] = None
    scheduled_end: Optional[datetime] = None
    chair_id: Optional[str] = None
    evolution_note: Optional[EvolutionNote] = None
```

### AppointmentUpdateIn
```python
class AppointmentUpdateIn(BaseModel):
    patient_id: Optional[str] = None
    doctor_id: Optional[str] = None
    estimate_id: Optional[str] = None
    service_id: Optional[str] = None
    scheduled_at: Optional[str] = None
    duration_minutes: Optional[int] = None
    status: Optional[str] = None
    notes: Optional[str] = None
    price_at_booking: Optional[float] = None
    amount_paid: Optional[float] = None
    payment_method: Optional[str] = None
    laboratory_cost: Optional[float] = None
    
    # Fase 5 additions
    scheduled_start: Optional[datetime] = None
    scheduled_end: Optional[datetime] = None
    chair_id: Optional[str] = None
    evolution_note: Optional[EvolutionNote] = None
    version: Optional[int] = None
```

### DashboardSummary
```python
class DashboardSummary(BaseModel):
    appointments_today: int
    appointments_upcoming_7d: int
    revenue_today: float
    revenue_month: float
    new_patients_month: int
    active_doctors: int
    total_patients: int
```

### InventoryItemType
```python
class InventoryItemType:
    MATERIAL = "material"
    UTENSILIO = "utensilio"
```

### InventoryItem
```python
class InventoryItem(BaseDocument):
    key_number: Optional[int] = None
    name: str
    unit: str = "piezas"
    entrada: float = 0.0
    salida: float = 0.0
    stock: float = 0.0
    min_stock: float = 5.0
    observations: Optional[str] = None
    expiration_date: Optional[str] = None  # e.g., "2027/01" or "2026/01"
    item_type: str = InventoryItemType.MATERIAL
    is_active: bool = True
```

### InventoryItemIn
```python
class InventoryItemIn(BaseModel):
    key_number: Optional[int] = None
    name: str
    unit: str = "piezas"
    entrada: float = 0.0
    salida: float = 0.0
    stock: float = 0.0
    min_stock: float = 5.0
    observations: Optional[str] = None
    expiration_date: Optional[str] = None
    item_type: str = InventoryItemType.MATERIAL
```

### InventoryItemUpdateIn
```python
class InventoryItemUpdateIn(BaseModel):
    key_number: Optional[int] = None
    name: Optional[str] = None
    unit: Optional[str] = None
    entrada: Optional[float] = None
    salida: Optional[float] = None
    stock: Optional[float] = None
    min_stock: Optional[float] = None
    observations: Optional[str] = None
    expiration_date: Optional[str] = None
    item_type: Optional[str] = None
    is_active: Optional[bool] = None
```

### InventoryTransaction
```python
class InventoryTransaction(BaseDocument):
    item_id: str
    item_name: str
    quantity_change: float
    previous_quantity: float
    new_quantity: float
    reason: str
    notes: Optional[str] = None
    performed_by: str
```

### StockAdjustmentIn
```python
class StockAdjustmentIn(BaseModel):
    quantity_change: float
    reason: str
    notes: Optional[str] = None
```

### CashRegisterStatus
```python
class CashRegisterStatus:
    OPEN = "open"
    CLOSED = "closed"
```

### CashRegister
```python
class CashRegister(BaseDocument):
    opened_at: str  # ISO datetime string
    closed_at: Optional[str] = None  # ISO datetime string
    opened_by_user_id: str
    closed_by_user_id: Optional[str] = None
    opening_balance: float
    expected_cash: float = 0.0
    counted_cash: Optional[float] = None
    difference: Optional[float] = None
    status: str = CashRegisterStatus.OPEN  # open/closed
    notes: Optional[str] = None
```

### CashRegisterOpenIn
```python
class CashRegisterOpenIn(BaseModel):
    opening_balance: float
    notes: Optional[str] = None
```

### CashRegisterCloseIn
```python
class CashRegisterCloseIn(BaseModel):
    counted_cash: float
    counted_by_method: Dict[str, float] = Field(default_factory=dict)
    notes: Optional[str] = None
```

### PaymentStatus
```python
class PaymentStatus:
    COMPLETED = "completed"
    CANCELLED = "cancelled"
    PARTIAL = "partial"
```

### PaymentMethod
```python
class PaymentMethod(BaseModel):
    type: str  # efectivo, tarjeta, transferencia
    amount: float
    reference: Optional[str] = None
    bank: Optional[str] = None
    last_four_digits: Optional[str] = None
    authorization_code: Optional[str] = None
```

### PaymentItem
```python
class PaymentItem(BaseModel):
    service_id: Optional[str] = None
    description: str
    quantity: int = 1
    unit_price: float
    discount: float = 0.0  # discount amount
    total: float
```

### Payment
```python
class Payment(BaseDocument):
    receipt_number: str  # unique folio
    patient_id: str
    appointment_id: Optional[str] = None
    doctor_id: Optional[str] = None
    payment_plan_id: Optional[str] = None
    estimate_id: Optional[str] = None
    total_amount: float
    paid_amount: float
    remaining_balance: float
    status: str = PaymentStatus.COMPLETED
    items: list[PaymentItem] = []
    methods: list[PaymentMethod] = []
    created_at: str  # ISO datetime string
    created_by_user_id: str
    financial_tracking_mode: Optional[str] = None
    laboratory_amount: float = 0.0
    clinical_amount: float = 0.0
    commission_rate: Optional[float] = None
    commission_earned: float = 0.0
    policy_version: Optional[str] = None
    idempotency_key: Optional[str] = None
```

### PaymentIn
```python
class PaymentIn(BaseModel):
    patient_id: str
    appointment_id: Optional[str] = None
    doctor_id: Optional[str] = None
    payment_plan_id: Optional[str] = None
    estimate_id: Optional[str] = None
    total_amount: float
    paid_amount: float
    remaining_balance: float
    items: list[PaymentItem]
    methods: list[PaymentMethod]
    financial_tracking_mode: Optional[str] = None
    idempotency_key: Optional[str] = None
```

### CashMovementType
```python
class CashMovementType:
    COBRO = "cobro"
    ANTICIPO = "anticipo"
    ABONO = "abono"
    DEVOLUCION = "devolucion"
    GASTO_MENOR = "gasto_menor"
    RETIRO = "retiro"
    AJUSTE = "ajuste"
    REVERSO = "reverso"
```

### CashMovementDirection
```python
class CashMovementDirection:
    IN = "in"
    OUT = "out"
```

### CashMovement
```python
class CashMovement(BaseDocument):
    cash_register_id: str
    payment_id: Optional[str] = None
    type: str  # CashMovementType
    payment_method: str  # efectivo, tarjeta, transferencia, mixto
    amount: float
    direction: str  # CashMovementDirection
    status: str = "active"  # active, cancelled
    concept: Optional[str] = None
    business_date: Optional[str] = None
    beneficiary: Optional[str] = None
    notes: Optional[str] = None
    created_at: str  # ISO datetime string
    created_by_user_id: str
```

### CashMovementIn
```python
class CashMovementIn(BaseModel):
    type: str  # Gasto menor, retiro, ajuste, etc.
    payment_method: str = "efectivo"
    amount: float
    concept: Optional[str] = None
    business_date: Optional[str] = None
    beneficiary: Optional[str] = None
    notes: Optional[str] = None
```

### MonthlyFinancialClosure
```python
class MonthlyFinancialClosure(BaseDocument):
    """Auditable monthly financial snapshot imported from a source workbook.

    This is intentionally separate from operational payments and cash movements:
    historical workbooks do not provide safe patient/appointment identifiers.
    """

    period_key: str  # YYYY-MM
    month_start: str
    month_end: str
    cutoff_date: str
    monthly_income: float = 0.0
    fixed_expenses: float = 0.0
    variable_expenses: float = 0.0
    doctor_payments: float = 0.0
    reported_profit: Optional[float] = None
    payment_methods: Dict[str, float] = Field(default_factory=dict)
    doctor_payment_breakdown: List[dict] = Field(default_factory=list)
    fixed_expense_breakdown: List[dict] = Field(default_factory=list)
    variable_expense_breakdown: List[dict] = Field(default_factory=list)
    source_file: str
    source_file_sha256: str
    source_variant: str = "primary"
    source_sheets: List[str] = Field(default_factory=list)
    source_row_counts: Dict[str, int] = Field(default_factory=dict)
    observations: List[str] = Field(default_factory=list)
    warnings: List[str] = Field(default_factory=list)
    review_comment: Optional[str] = None
    status: str = "pending_review"  # pending_review | approved | imported | quarantined
    import_batch_id: str
    schema_version: str = "monthly-financial-closure-v1"
```

### RefundOrReversal
```python
class RefundOrReversal(BaseDocument):
    payment_id: str
    amount: float
    reason: str
    authorized_by_user_id: str
    created_by_user_id: str
    created_at: str
    idempotency_key: Optional[str] = None
```

### TreatmentPhase
```python
class TreatmentPhase(BaseModel):
    id: str
    name: str
    description: Optional[str] = None
    order: int
    total: float
    status: str = "pending"
```

### ClubEstimateContext
```python
class ClubEstimateContext(BaseModel):
    membership_id: str
    company_id: str
    company_name: str
    agreement_id: str
    agreement_folio: str
    agreement_name: Optional[str] = None
    member_type: str
    member_name: Optional[str] = None
    employee_number: Optional[str] = None
    relationship: Optional[str] = None
    holder_membership_id: Optional[str] = None
    holder_name: Optional[str] = None
    valid_until: Optional[str] = None
```

### TreatmentEstimateItem
```python
class TreatmentEstimateItem(BaseModel):
    id: str
    service_id: Optional[str] = None
    phase_id: Optional[str] = None
    tooth_number: Optional[str] = None
    area: Optional[str] = None
    description: str
    quantity: int = 1
    unit_price: float
    discount: float = 0.0
    total: float
    doctor_id: Optional[str] = None
    status: str = "pending"  # pending, authorized, programmed, in_process, completed, cancelled
    notes: Optional[str] = None
    laboratory_cost: float = 0.0
    order: int = 0
    # Club Sonrisas historical snapshot fields (backward compatible with legacy items)
    pricing_source: str = "standard"  # "standard" | "club_sonrisas"
    public_unit_price: Optional[float] = None
    club_applied: bool = False
    club_rule_id: Optional[str] = None
    club_rule_type: Optional[str] = None
    club_company_id: Optional[str] = None
    club_agreement_id: Optional[str] = None
    club_membership_id: Optional[str] = None
    club_discount_amount: float = 0.0
    club_discount_percentage: float = 0.0
    club_max_uses_per_member: Optional[int] = None
    club_applicable_member_types: Optional[List[str]] = None
    club_combinable_with_promotions: bool = False
```

### TreatmentEstimate
```python
class TreatmentEstimate(BaseDocument):
    folio: str  # unique PRES-YYYYMMDD-XXXX
    patient_id: str
    responsible_doctor_id: Optional[str] = None
    created_by_user_id: str
    created_at: str
    valid_until: str
    diagnosis: Optional[str] = None
    notes: Optional[str] = None
    subtotal: float
    discount_total: float
    total: float
    deposit_required: float = 0.0
    financed_amount: float = 0.0
    paid_amount: float = 0.0
    balance: float = 0.0
    status: str = "draft"  # draft, presented, accepted, rejected, partial_accepted, in_treatment, completed, cancelled, expired
    accepted_at: Optional[str] = None
    accepted_by_name: Optional[str] = None
    authorized_by_user_id: Optional[str] = None
    items: list[TreatmentEstimateItem] = []
    phases: list[TreatmentPhase] = []
    # Club Sonrisas historical summary & context snapshot
    public_subtotal: Optional[float] = None
    club_benefit_total: float = 0.0
    club_context: Optional[ClubEstimateContext] = None
    laboratory_total: float = 0.0
    laboratory_paid: float = 0.0
    clinical_paid: float = 0.0
    commission_earned: float = 0.0
    commission_paid: float = 0.0
    financial_tracking_mode: Optional[str] = None
    doctor_transfer_history: list[dict] = []
    commission_shares: list[dict] = []
```

### TreatmentEstimateIn
```python
class TreatmentEstimateIn(BaseModel):
    patient_id: str
    responsible_doctor_id: Optional[str] = None
    valid_until: str
    diagnosis: Optional[str] = None
    notes: Optional[str] = None
    subtotal: float
    discount_total: float
    total: float
    deposit_required: float = 0.0
    financed_amount: float = 0.0
    items: list[TreatmentEstimateItem]
    phases: list[TreatmentPhase] = []
    public_subtotal: Optional[float] = None
    club_benefit_total: Optional[float] = None
    club_context: Optional[ClubEstimateContext] = None
    commission_shares: list[dict] = []
```

### TreatmentCloseIn
```python
class TreatmentCloseIn(BaseModel):
    allow_exception: bool = False
    reason: Optional[str] = None
    expected_version: Optional[int] = None
```

### PaymentPlanInstallment
```python
class PaymentPlanInstallment(BaseModel):
    id: str
    installment_number: int
    due_date: str
    scheduled_amount: float
    paid_amount: float = 0.0
    remaining_amount: float
    status: str = "pending"
```

### PaymentPlan
```python
class PaymentPlan(BaseDocument):
    estimate_id: str
    patient_id: str
    responsible_payer_name: str
    responsible_payer_phone: str
    total_amount: float
    deposit: float
    financed_amount: float
    frequency: str  # single, monthly, biweekly, weekly, custom
    number_of_payments: int
    start_date: str
    status: str = "pending"  # pending, active, completed, cancelled
    created_at: str
    accepted_at: Optional[str] = None
    installments: list[PaymentPlanInstallment] = []
    financial_tracking_mode: Optional[str] = None
    laboratory_total: float = 0.0
    laboratory_paid: float = 0.0
    clinical_paid: float = 0.0
    commission_earned: float = 0.0
    commission_paid: float = 0.0
```

### PaymentPlanIn
```python
class PaymentPlanIn(BaseModel):
    estimate_id: str
    patient_id: str
    responsible_payer_name: str
    responsible_payer_phone: str
    total_amount: float
    deposit: float
    financed_amount: float
    frequency: str
    number_of_payments: int
    start_date: str
    installments: list[PaymentPlanInstallment]
```

### PaymentAgreement
```python
class PaymentAgreement(BaseDocument):
    folio: str  # CONV-YYYYMMDD-XXXX
    estimate_id: str
    payment_plan_id: str
    patient_id: str
    responsible_payer_name: str
    terms_version: str = "1.0"
    accepted_at: str
    patient_signature: str
    clinic_signature: str
    created_at: str
```

### CommissionPayout
```python
class CommissionPayout(BaseDocument):
    doctor_id: str
    amount: float
    period_from: str
    period_to: str
    notes: Optional[str] = None
    authorized_by_user_id: str
    status: str = "authorized"  # authorized | paid | voided | adjusted
    authorized_at: Optional[str] = None
    paid_by_user_id: Optional[str] = None
    paid_at: Optional[str] = None
    idempotency_key: Optional[str] = None
    policy_version: Optional[str] = None
```

### LaboratoryOrder
```python
class LaboratoryOrder(BaseDocument):
    patient_id: str
    estimate_id: Optional[str] = None
    payment_plan_id: Optional[str] = None
    appointment_id: Optional[str] = None
    provider_id: Optional[str] = None
    description: str
    agreed_cost: float
    required_advance: float = 0.0
    advance_paid: float = 0.0
    status: str = "draft"  # draft | authorized_with_warning | sent | in_process | received | completed | cancelled
    insufficient_advance_warning: bool = False
    authorized_by_user_id: Optional[str] = None
    authorized_at: Optional[str] = None
    authorization_reason: Optional[str] = None
    created_by_user_id: str
    version: int = 1
```

### LaboratoryOrderIn
```python
class LaboratoryOrderIn(BaseModel):
    patient_id: str
    estimate_id: Optional[str] = None
    payment_plan_id: Optional[str] = None
    appointment_id: Optional[str] = None
    provider_id: Optional[str] = None
    description: str
    agreed_cost: float
    required_advance: float = 0.0
    advance_paid: float = 0.0
    authorize_insufficient_advance: bool = False
    authorization_reason: Optional[str] = None
```

### LaboratoryProviderPayment
```python
class LaboratoryProviderPayment(BaseDocument):
    laboratory_order_id: str
    provider_id: Optional[str] = None
    amount: float
    payment_method: str = "efectivo"
    cash_movement_id: Optional[str] = None
    status: str = "active"
    paid_at: str
    paid_by_user_id: str
    idempotency_key: Optional[str] = None
    reversal_reason: Optional[str] = None
```

### FollowUpMethod
```python
class FollowUpMethod:
    WHATSAPP = "whatsapp"
    CALL = "llamada"
```

### FollowUpOutcome
```python
class FollowUpOutcome:
    NO_RESPONSE = "sin_respuesta"
    CONTACTED = "contactado"
    WANTS_APPOINTMENT = "quiere_cita"
    APPOINTMENT_SCHEDULED = "cita_agendada"
    NOT_INTERESTED = "no_interesado"
```

### PatientFollowUp
```python
class PatientFollowUp(BaseDocument):
    patient_id: str
    contacted_at: datetime = Field(default_factory=utcnow)
    user_id: str
    user_name: str
    user_role: str
    method: str  # "whatsapp" | "llamada"
    outcome: str  # "sin_respuesta" | "contactado" | "quiere_cita" | "cita_agendada" | "no_interesado"
    notes: Optional[str] = None
    next_follow_up_date: Optional[str] = None  # YYYY-MM-DD
    appointment_id: Optional[str] = None
```

### PatientFollowUpIn
```python
class PatientFollowUpIn(BaseModel):
    method: str
    outcome: str
    notes: Optional[str] = None
    next_follow_up_date: Optional[str] = None  # YYYY-MM-DD
    appointment_id: Optional[str] = None
```

### PatientFollowUpUpdateIn
```python
class PatientFollowUpUpdateIn(BaseModel):
    outcome: Optional[str] = None
    notes: Optional[str] = None
    next_follow_up_date: Optional[str] = None  # YYYY-MM-DD or "" to clear
    appointment_id: Optional[str] = None
```

### ClubCompanyStatus
```python
class ClubCompanyStatus:
    PROSPECT = "prospect"
    ACTIVE = "active"
    SUSPENDED = "suspended"
    EXPIRED = "expired"
    CANCELLED = "cancelled"
```

### ClubCompany
```python
class ClubCompany(BaseDocument):
    commercial_name: str
    legal_name: Optional[str] = None
    rfc: Optional[str] = None
    fiscal_address: Optional[str] = None
    contact_name: Optional[str] = None
    contact_position: Optional[str] = None
    contact_email: Optional[EmailStr] = None
    contact_phone: Optional[str] = None
    employee_count: Optional[int] = None
    status: str = ClubCompanyStatus.PROSPECT
    notes: Optional[str] = None
    created_by_user_id: Optional[str] = None
    version: int = 1
```

### ClubCompanyIn
```python
class ClubCompanyIn(BaseModel):
    commercial_name: str
    legal_name: Optional[str] = None
    rfc: Optional[str] = None
    fiscal_address: Optional[str] = None
    contact_name: Optional[str] = None
    contact_position: Optional[str] = None
    contact_email: Optional[EmailStr] = None
    contact_phone: Optional[str] = None
    employee_count: Optional[int] = None
    status: str = ClubCompanyStatus.PROSPECT
    notes: Optional[str] = None
```

### ClubCompanyUpdateIn
```python
class ClubCompanyUpdateIn(BaseModel):
    commercial_name: Optional[str] = None
    legal_name: Optional[str] = None
    rfc: Optional[str] = None
    fiscal_address: Optional[str] = None
    contact_name: Optional[str] = None
    contact_position: Optional[str] = None
    contact_email: Optional[EmailStr] = None
    contact_phone: Optional[str] = None
    employee_count: Optional[int] = None
    status: Optional[str] = None
    notes: Optional[str] = None
```

### ClubAgreementStatus
```python
class ClubAgreementStatus:
    DRAFT = "draft"
    ACTIVE = "active"
    SUSPENDED = "suspended"
    EXPIRED = "expired"
    CANCELLED = "cancelled"
```

### ClubAgreement
```python
class ClubAgreement(BaseDocument):
    company_id: str
    folio: str  # CS-YYYY-0001
    name: str
    start_date: str  # YYYY-MM-DD
    end_date: Optional[str] = None  # YYYY-MM-DD or None
    status: str = ClubAgreementStatus.DRAFT
    max_beneficiaries: int = 5
    allowed_relationships: Optional[List[str]] = None
    payer_type: str = "patient"  # "patient" | "company"
    notes: Optional[str] = None
    terms: Optional[str] = None
    created_by_user_id: Optional[str] = None
    version: int = 1
```

### ClubAgreementIn
```python
class ClubAgreementIn(BaseModel):
    company_id: str
    name: str
    start_date: str  # YYYY-MM-DD
    end_date: Optional[str] = None
    status: str = ClubAgreementStatus.DRAFT
    max_beneficiaries: int = 5
    allowed_relationships: Optional[List[str]] = None
    payer_type: str = "patient"
    notes: Optional[str] = None
    terms: Optional[str] = None
```

### ClubAgreementUpdateIn
```python
class ClubAgreementUpdateIn(BaseModel):
    name: Optional[str] = None
    start_date: Optional[str] = None
    end_date: Optional[str] = None
    status: Optional[str] = None
    max_beneficiaries: Optional[int] = None
    allowed_relationships: Optional[List[str]] = None
    payer_type: Optional[str] = None
    notes: Optional[str] = None
    terms: Optional[str] = None
```

### ClubMemberType
```python
class ClubMemberType:
    EMPLOYEE = "employee"
    BENEFICIARY = "beneficiary"
```

### ClubMembershipStatus
```python
class ClubMembershipStatus:
    ACTIVE = "active"
    SUSPENDED = "suspended"
    INACTIVE = "inactive"
```

### ClubRelationship
```python
class ClubRelationship:
    SPOUSE = "spouse"
    PARTNER = "partner"  # Concubino(a) / Pareja
    CHILD = "child"
    FATHER = "father"
    MOTHER = "mother"
    SIBLING = "sibling"
    OTHER = "other"
```

### ClubMembership
```python
class ClubMembership(BaseDocument):
    company_id: str
    agreement_id: str
    member_type: str  # "employee" | "beneficiary"
    employee_number: Optional[str] = None
    first_name: str
    last_name: str
    second_last_name: Optional[str] = None
    phone: Optional[str] = None
    email: Optional[EmailStr] = None
    date_of_birth: Optional[str] = None  # YYYY-MM-DD
    relationship: Optional[str] = None  # spouse, child, father, mother, sibling, other
    holder_membership_id: Optional[str] = None
    patient_id: Optional[str] = None
    status: str = ClubMembershipStatus.ACTIVE
    joined_at: str = Field(default_factory=lambda: utcnow().date().isoformat())
    ended_at: Optional[str] = None
    notes: Optional[str] = None
    created_by_user_id: Optional[str] = None
    beneficiaries_version: int = 1
    version: int = 1
```

### ClubMembershipIn
```python
class ClubMembershipIn(BaseModel):
    company_id: str
    agreement_id: str
    member_type: str
    employee_number: Optional[str] = None
    first_name: str
    last_name: str
    second_last_name: Optional[str] = None
    phone: Optional[str] = None
    email: Optional[EmailStr] = None
    date_of_birth: Optional[str] = None
    relationship: Optional[str] = None
    holder_membership_id: Optional[str] = None
    patient_id: Optional[str] = None
    status: str = ClubMembershipStatus.ACTIVE
    joined_at: Optional[str] = None
    ended_at: Optional[str] = None
    notes: Optional[str] = None
```

### ClubMembershipUpdateIn
```python
class ClubMembershipUpdateIn(BaseModel):
    agreement_id: Optional[str] = None
    employee_number: Optional[str] = None
    first_name: Optional[str] = None
    last_name: Optional[str] = None
    second_last_name: Optional[str] = None
    phone: Optional[str] = None
    email: Optional[EmailStr] = None
    date_of_birth: Optional[str] = None
    relationship: Optional[str] = None
    status: Optional[str] = None
    joined_at: Optional[str] = None
    ended_at: Optional[str] = None
    notes: Optional[str] = None
```

### ClubMemberLinkPatientIn
```python
class ClubMemberLinkPatientIn(BaseModel):
    patient_id: str
```

### ClubPriceRuleType
```python
class ClubPriceRuleType:
    FIXED_PRICE = "fixed_price"
    PERCENTAGE_DISCOUNT = "percentage_discount"
    AMOUNT_DISCOUNT = "amount_discount"
    FREE = "free"
```

### ClubPriceRule
```python
class ClubPriceRule(BaseDocument):
    agreement_id: Optional[str] = None
    company_id: Optional[str] = None
    service_id: str
    rule_type: str  # fixed_price | percentage_discount | amount_discount | free
    value: float = 0.0
    priority: int = 0
    applicable_member_types: List[str] = Field(default_factory=lambda: ["employee", "beneficiary"])
    max_uses_per_member: Optional[int] = None
    combinable_with_promotions: bool = False
    is_active: bool = True
    valid_from: Optional[str] = None  # YYYY-MM-DD
    valid_until: Optional[str] = None  # YYYY-MM-DD
    notes: Optional[str] = None
    created_by_user_id: Optional[str] = None
    version: int = 1
```

### ClubPriceRuleIn
```python
class ClubPriceRuleIn(BaseModel):
    agreement_id: Optional[str] = None
    company_id: Optional[str] = None
    service_id: str
    rule_type: str
    value: float = 0.0
    priority: Optional[int] = None
    applicable_member_types: Optional[List[str]] = None
    max_uses_per_member: Optional[int] = None
    combinable_with_promotions: Optional[bool] = False
    is_active: bool = True
    valid_from: Optional[str] = None
    valid_until: Optional[str] = None
    notes: Optional[str] = None
```

### ClubPriceRuleUpdateIn
```python
class ClubPriceRuleUpdateIn(BaseModel):
    rule_type: Optional[str] = None
    value: Optional[float] = None
    priority: Optional[int] = None
    applicable_member_types: Optional[List[str]] = None
    max_uses_per_member: Optional[int] = None
    combinable_with_promotions: Optional[bool] = None
    is_active: Optional[bool] = None
    valid_from: Optional[str] = None
    valid_until: Optional[str] = None
    notes: Optional[str] = None
```

### ClubBenefitUsageStatus
```python
class ClubBenefitUsageStatus:
    RESERVED = "reserved"
    CONSUMED = "consumed"
    CANCELLED = "cancelled"
```

### ClubBenefitUsage
```python
class ClubBenefitUsage(BaseDocument):
    membership_id: str
    price_rule_id: str
    service_id: str
    estimate_id: Optional[str] = None
    payment_id: Optional[str] = None
    cash_movement_id: Optional[str] = None
    status: str = ClubBenefitUsageStatus.RESERVED
    notes: Optional[str] = None
    used_at: Optional[str] = None
    created_by_user_id: Optional[str] = None
```

### ClubBenefitUsageIn
```python
class ClubBenefitUsageIn(BaseModel):
    membership_id: str
    price_rule_id: str
    service_id: str
    estimate_id: Optional[str] = None
    payment_id: Optional[str] = None
    cash_movement_id: Optional[str] = None
    status: str = ClubBenefitUsageStatus.RESERVED
    notes: Optional[str] = None
    used_at: Optional[str] = None
```

### ClubResolveBatchIn
```python
class ClubResolveBatchIn(BaseModel):
    service_ids: List[str] = Field(default_factory=list)
    membership_id: Optional[str] = None
    patient_id: Optional[str] = None
    date: Optional[str] = None
```

## Anexo de operaciones HTTP observadas
Los controles por recurso y rol internos también aplican; este índice no sustituye autorización.

### appointments_router.py

- router.get("") — list_appointments
```python
async def list_appointments(
    request: Request,
    background_tasks: BackgroundTasks,
    date_from: str | None = Query(None, description="YYYY-MM-DD"),
    date_to: str | None = Query(None, description="YYYY-MM-DD"),
    doctor_id: str | None = None,
    patient_id: str | None = None,
    status: str | None = None,
    sync: bool = Query(False, description="Trigger background sync with Google"),
    user: dict = Depends(get_current_user),
):
```

- router.post("") — create_appointment
```python
async def create_appointment(
    payload: AppointmentIn,
    request: Request,
    background_tasks: BackgroundTasks,
    user: dict = Depends(
        require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION, UserRole.DOCTOR)
    ),
):
```

- router.put("/{appointment_id}") — update_appointment
```python
async def update_appointment(
    appointment_id: str,
    payload: AppointmentUpdateIn,
    request: Request,
    background_tasks: BackgroundTasks,
    user: dict = Depends(
        require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION, UserRole.DOCTOR)
    ),
    version: Optional[int] = Query(None, description="Expected appointment version for concurrency")
):
```

- router.delete("/{appointment_id}") — delete_appointment
```python
async def delete_appointment(
    appointment_id: str,
    request: Request,
    background_tasks: BackgroundTasks,
    _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- router.get("/{appointment_id}/evolution-note") — get_evolution_note
```python
async def get_evolution_note(
    appointment_id: str,
    request: Request,
    user: dict = Depends(get_current_user)
):
```

- router.put("/{appointment_id}/evolution-note") — update_evolution_note
```python
async def update_evolution_note(
    appointment_id: str,
    payload: EvolutionNoteUpdateIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.DOCTOR)),
    version: int = Query(..., description="Expected appointment version"),
    note_version: int = Query(..., description="Expected evolution note version")
):
```

- router.post("/{appointment_id}/evolution-note/doctor-confirmation") — doctor_confirmation
```python
async def doctor_confirmation(
    appointment_id: str,
    payload: DoctorConfirmationIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.DOCTOR))
):
```

- router.post("/{appointment_id}/evolution-note/patient-confirmation") — patient_confirmation
```python
async def patient_confirmation(
    appointment_id: str,
    payload: PatientConfirmationIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION, UserRole.DOCTOR))
):
```

- router.post("/{appointment_id}/exceptional-close") — exceptional_close
```python
async def exceptional_close(
    appointment_id: str,
    payload: ExceptionalCloseIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN))
):
```

- router.post("/{appointment_id}/cancel") — cancel_appointment
```python
async def cancel_appointment(
    appointment_id: str,
    payload: AppointmentCancelIn,
    request: Request,
    background_tasks: BackgroundTasks,
    user: dict = Depends(get_current_user)
):
```

- router.post("/{appointment_id}/reschedule") — reschedule_appointment
```python
async def reschedule_appointment(
    appointment_id: str,
    payload: AppointmentRescheduleIn,
    request: Request,
    background_tasks: BackgroundTasks,
    user: dict = Depends(get_current_user)
):
```

### auth_router.py

- router.post("/login") — login
```python
async def login(payload: LoginIn, request: Request, response: Response):
```

- router.post("/logout") — logout
```python
async def logout(response: Response, user: dict = Depends(get_current_user)):
```

- router.get("/me") — me
```python
async def me(user: dict = Depends(get_current_user)):
```

- router.post("/refresh") — refresh
```python
async def refresh(request: Request, response: Response):
```

### calendar_router.py

- router.get("/authorize") — authorize
```python
async def authorize(request: Request, _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN))):
```

- router.get("/callback") — callback
```python
async def callback(request: Request, code: str | None = None, state: str | None = None,
                   error: str | None = None):
```

- sync_router.get("/status") — status
```python
async def status(request: Request, _: dict = Depends(get_current_user)):
```

- sync_router.get("/calendars") — calendars
```python
async def calendars(request: Request, _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN))):
```

- sync_router.post("/disconnect") — do_disconnect
```python
async def do_disconnect(
    request: Request, _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN))
):
```

- sync_router.post("/create") — create_google_calendar
```python
async def create_google_calendar(
    payload: CreateCalendarIn,
    request: Request,
    _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- sync_router.post("/sync") — trigger_sync
```python
async def trigger_sync(request: Request, _: dict = Depends(get_current_user)):
```

### cash_register_router.py

- router.get("/current") — get_current_register
```python
async def get_current_register(request: Request, _: dict = Depends(get_current_user)):
```

- router.post("/open") — open_register
```python
async def open_register(
    payload: CashRegisterOpenIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- router.post("/close") — close_register
```python
async def close_register(
    payload: CashRegisterCloseIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- payments_router.get("/pending") — get_pending_payments
```python
async def get_pending_payments(request: Request, _: dict = Depends(get_current_user)):
```

- payments_router.post("") — create_payment
```python
async def create_payment(
    payload: PaymentIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- payments_router.post("/{payment_id}/allocations") — allocate_payment_across_treatments
```python
async def allocate_payment_across_treatments(
    payment_id: str,
    payload: PaymentAllocationRequest,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- payments_router.post("/{payment_id}/reverse") — reverse_payment
```python
async def reverse_payment(
    payment_id: str,
    payload: dict,  # contains "reason"
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- payments_router.get("/history") — get_payments_history
```python
async def get_payments_history(request: Request, _: dict = Depends(get_current_user)):
```

- router.get("/movements") — get_movements
```python
async def get_movements(
    request: Request,
    cash_register_id: Optional[str] = Query(None),
    _: dict = Depends(get_current_user),
):
```

- router.post("/movements") — create_manual_movement
```python
async def create_manual_movement(
    payload: CashMovementIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- payments_router.get("/receipts/{payment_id}") — get_receipt
```python
async def get_receipt(payment_id: str, request: Request, _: dict = Depends(get_current_user)):
```

- payments_router.post("/receipts/{payment_id}/send-whatsapp") — send_receipt_whatsapp
```python
async def send_receipt_whatsapp(payment_id: str, request: Request, _: dict = Depends(get_current_user)):
    # Mocking WhatsApp sending logic
```

### chairs_router.py

- router.get("/api/chairs") — list_chairs
```python
async def list_chairs(
    request: Request,
    active_only: bool = Query(True, description="Mostrar solo sillones activos"),
    user: dict = Depends(get_current_user),
):
```

- router.get("/api/chairs/{chair_id}") — get_chair
```python
async def get_chair(
    chair_id: str,
    request: Request,
    user: dict = Depends(get_current_user),
):
```

- router.post("/api/chairs") — create_chair
```python
async def create_chair(
    payload: DentalChairIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.patch("/api/chairs/{chair_id}") — update_chair
```python
async def update_chair(
    chair_id: str,
    payload: DentalChairUpdateIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
    version: int = Query(..., description="Expected chair version")
):
```

- router.patch("/api/chairs/{chair_id}/deactivate") — deactivate_chair
```python
async def deactivate_chair(
    chair_id: str,
    request: Request,
    motivo: str = Query(..., description="Motivo de la desactivación"),
    override: bool = Query(False, description="Saltar advertencia de citas futuras"),
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN))
):
```

- router.patch("/api/chairs/{chair_id}/activate") — activate_chair
```python
async def activate_chair(
    chair_id: str,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN))
):
```

- router.get("/api/clinic-closures") — list_closures
```python
async def list_closures(
    request: Request,
    active_only: bool = Query(True, description="Mostrar solo cierres activos"),
    user: dict = Depends(get_current_user),
):
```

- router.post("/api/clinic-closures") — create_closure
```python
async def create_closure(
    payload: ClinicClosureIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.patch("/api/clinic-closures/{closure_id}/deactivate") — deactivate_closure
```python
async def deactivate_closure(
    closure_id: str,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN))
):
```

- router.get("/api/chairs/{chair_id}/maintenance") — list_maintenance
```python
async def list_maintenance(
    chair_id: str,
    request: Request,
    user: dict = Depends(get_current_user)
):
```

- router.post("/api/chairs/{chair_id}/maintenance") — create_maintenance
```python
async def create_maintenance(
    chair_id: str,
    payload: ChairMaintenanceIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN))
):
```

- router.post("/api/chair-maintenance/{maintenance_id}/cancel") — cancel_maintenance
```python
async def cancel_maintenance(
    maintenance_id: str,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN))
):
```

- router.post("/api/chair-slots/reconcile") — reconcile_slots
```python
async def reconcile_slots(
    request: Request,
    dry_run: bool = Query(True, description="Si es true, no realiza modificaciones"),
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN))
):
```

- router.get("/api/reports/chair-occupancy") — chair_occupancy_report
```python
async def chair_occupancy_report(
    request: Request,
    date_from: str = Query(..., description="Fecha inicial local YYYY-MM-DD"),
    date_to: str = Query(..., description="Fecha final local YYYY-MM-DD"),
    chair_id: Optional[str] = Query(None, description="Filtrar por sillón"),
    doctor_id: Optional[str] = Query(None, description="Filtrar por doctor"),
    grouping: str = Query("day", description="Agrupamiento: day, week, month"),
    timezone_str: str = Query("America/Mexico_City", alias="timezone", description="Zona horaria (restringida a America/Mexico_City)"),
    user: dict = Depends(get_current_user)
):
```

### club_sonrisas_router.py

- router.get("/companies") — list_companies
```python
async def list_companies(
    request: Request,
    q: Optional[str] = Query(None, description="Buscar por nombre comercial, razón social, RFC o contacto"),
    status: Optional[str] = Query(None, description="Filtrar por estado"),
    page: Optional[int] = Query(None, ge=1),
    page_size: int = Query(25, ge=1, le=200),
    _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- router.post("/companies") — create_company
```python
async def create_company(
    payload: ClubCompanyIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.get("/companies/{company_id}") — get_company
```python
async def get_company(
    company_id: str,
    request: Request,
    _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- router.put("/companies/{company_id}") — update_company
```python
async def update_company(
    company_id: str,
    payload: ClubCompanyUpdateIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.delete("/companies/{company_id}") — delete_company
```python
async def delete_company(
    company_id: str,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER)),
):
```

- router.get("/agreements") — list_agreements
```python
async def list_agreements(
    request: Request,
    company_id: Optional[str] = Query(None),
    status: Optional[str] = Query(None),
    _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- router.post("/agreements") — create_agreement
```python
async def create_agreement(
    payload: ClubAgreementIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER)),
):
```

- router.get("/agreements/{agreement_id}") — get_agreement
```python
async def get_agreement(
    agreement_id: str,
    request: Request,
    _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- router.put("/agreements/{agreement_id}") — update_agreement
```python
async def update_agreement(
    agreement_id: str,
    payload: ClubAgreementUpdateIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER)),
):
```

- router.delete("/agreements/{agreement_id}") — delete_agreement
```python
async def delete_agreement(
    agreement_id: str,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER)),
):
```

- router.get("/members") — list_members
```python
async def list_members(
    request: Request,
    company_id: Optional[str] = Query(None),
    agreement_id: Optional[str] = Query(None),
    member_type: Optional[str] = Query(None),
    status: Optional[str] = Query(None),
    q: Optional[str] = Query(None, description="Buscar por nombre, apellidos, teléfono, correo o número de empleado"),
    employee_number: Optional[str] = Query(None),
    patient_id: Optional[str] = Query(None),
    holder_membership_id: Optional[str] = Query(None),
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION, UserRole.DOCTOR)),
):
```

- router.post("/members") — create_member
```python
async def create_member(
    payload: ClubMembershipIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.get("/members/by-patient/{patient_id}") — get_member_by_patient
```python
async def get_member_by_patient(
    patient_id: str,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION, UserRole.DOCTOR)),
):
```

- router.get("/members/{membership_id}") — get_member
```python
async def get_member(
    membership_id: str,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION, UserRole.DOCTOR)),
):
```

- router.put("/members/{membership_id}") — update_member
```python
async def update_member(
    membership_id: str,
    payload: ClubMembershipUpdateIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.delete("/members/{membership_id}") — delete_member
```python
async def delete_member(
    membership_id: str,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.post("/members/{membership_id}/link-patient") — link_member_to_patient
```python
async def link_member_to_patient(
    membership_id: str,
    payload: ClubMemberLinkPatientIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- router.get("/members/{membership_id}/eligibility") — get_member_eligibility
```python
async def get_member_eligibility(
    membership_id: str,
    request: Request,
    date: Optional[str] = Query(None, description="Fecha de evaluación YYYY-MM-DD (default: hoy)"),
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION, UserRole.DOCTOR)),
):
```

- router.get("/pricing/resolve") — resolve_pricing_endpoint
```python
async def resolve_pricing_endpoint(
    request: Request,
    service_id: str = Query(..., description="ID del servicio en el catálogo"),
    membership_id: Optional[str] = Query(None, description="ID de la membresía Club Sonrisas"),
    patient_id: Optional[str] = Query(None, description="ID del paciente (alternativo a membership_id)"),
    date: Optional[str] = Query(None, description="Fecha de evaluación YYYY-MM-DD (default: hoy)"),
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION, UserRole.DOCTOR)),
):
```

- router.post("/pricing/resolve-batch") — resolve_pricing_batch_endpoint
```python
async def resolve_pricing_batch_endpoint(
    payload: ClubResolveBatchIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION, UserRole.DOCTOR)),
):
```

- router.get("/price-rules") — list_price_rules
```python
async def list_price_rules(
    request: Request,
    company_id: Optional[str] = Query(None),
    agreement_id: Optional[str] = Query(None),
    service_id: Optional[str] = Query(None),
    is_active: Optional[bool] = Query(None),
    _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- router.post("/price-rules") — create_price_rule
```python
async def create_price_rule(
    payload: ClubPriceRuleIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.get("/price-rules/{rule_id}") — get_price_rule
```python
async def get_price_rule(
    rule_id: str,
    request: Request,
    _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- router.put("/price-rules/{rule_id}") — update_price_rule
```python
async def update_price_rule(
    rule_id: str,
    payload: ClubPriceRuleUpdateIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.delete("/price-rules/{rule_id}") — delete_price_rule
```python
async def delete_price_rule(
    rule_id: str,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.get("/usages") — list_benefit_usages
```python
async def list_benefit_usages(
    request: Request,
    membership_id: Optional[str] = Query(None),
    price_rule_id: Optional[str] = Query(None),
    service_id: Optional[str] = Query(None),
    estimate_id: Optional[str] = Query(None),
    status: Optional[str] = Query(None),
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION, UserRole.DOCTOR)),
):
```

- router.post("/usages") — create_benefit_usage
```python
async def create_benefit_usage(
    payload: ClubBenefitUsageIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- router.get("/analytics") — club_sonrisas_analytics
```python
async def club_sonrisas_analytics(
    request: Request,
    start_date: Optional[str] = Query(None, description="Inicio YYYY-MM-DD (default: 12 meses atrás)"),
    end_date: Optional[str] = Query(None, description="Fin YYYY-MM-DD (default: hoy)"),
    company_id: Optional[str] = Query(None),
    _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

### commissions_router.py

- router.get("") — get_commissions_dashboard
```python
async def get_commissions_dashboard(
    request: Request,
    date_from: str = Query(..., description="YYYY-MM-DD"),
    date_to: str = Query(..., description="YYYY-MM-DD"),
    doctor_id: str | None = None,
    doctor_type: str | None = None,
    _: dict = Depends(require_role(UserRole.OWNER)),
):
```

- router.get("/doctor/{doctor_id}") — get_doctor_commission_detail
```python
async def get_doctor_commission_detail(
    doctor_id: str,
    request: Request,
    date_from: str = Query(..., description="YYYY-MM-DD"),
    date_to: str = Query(..., description="YYYY-MM-DD"),
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.DOCTOR)),
):
```

- router.get("/rules") — get_rules_endpoint
```python
async def get_rules_endpoint(
    request: Request,
    _: dict = Depends(require_role(UserRole.OWNER)),
):
```

- router.put("/rules") — update_rules_endpoint
```python
async def update_rules_endpoint(
    rules_list: list,
    request: Request,
    _: dict = Depends(require_role(UserRole.OWNER)),
):
```

- router.get("/payment-fee-rule") — get_payment_fee_rule
```python
async def get_payment_fee_rule(request: Request, _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN))):
```

- router.put("/payment-fee-rule") — update_payment_fee_rule
```python
async def update_payment_fee_rule(
    payload: dict,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER)),
):
```

- router.post("/payouts") — record_commission_payout
```python
async def record_commission_payout(
    payload: dict,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.post("/payouts/{payout_id}/pay") — pay_commission_payout
```python
async def pay_commission_payout(
    payout_id: str,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER)),
):
```

### dashboard_router.py

- router.get("/summary") — summary
```python
async def summary(request: Request, user: dict = Depends(get_current_user)):
```

- router.get("/today") — today_appointments
```python
async def today_appointments(request: Request, user: dict = Depends(get_current_user)):
```

- router.get("/reports") — get_backoffice_reports
```python
async def get_backoffice_reports(
    request: Request,
    date_from: Optional[str] = Query(None),
    date_to: Optional[str] = Query(None),
    user: dict = Depends(get_current_user)
):
```

### doctors_router.py

- router.get("") — list_doctors
```python
async def list_doctors(request: Request, _: dict = Depends(get_current_user)):
```

- router.post("") — create_doctor
```python
async def create_doctor(
    payload: DoctorIn,
    request: Request,
    background_tasks: BackgroundTasks,
    _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.put("/{doctor_id}") — update_doctor
```python
async def update_doctor(
    doctor_id: str,
    payload: DoctorIn,
    request: Request,
    background_tasks: BackgroundTasks,
    _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.post("/{doctor_id}/sync-calendar") — sync_single_doctor_calendar
```python
async def sync_single_doctor_calendar(
    doctor_id: str,
    request: Request,
    _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.delete("/{doctor_id}") — delete_doctor
```python
async def delete_doctor(
    doctor_id: str,
    request: Request,
    _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

### estimates_router.py

- router.get("") — get_estimates
```python
async def get_estimates(
    request: Request,
    patient_id: Optional[str] = Query(None),
    status: Optional[str] = Query(None),
    user: dict = Depends(get_current_user),
):
```

- router.post("") — create_estimate
```python
async def create_estimate(
    payload: TreatmentEstimateIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- router.get("/{estimate_id}") — get_estimate
```python
async def get_estimate(estimate_id: str, request: Request, user: dict = Depends(get_current_user)):
```

- router.put("/{estimate_id}") — update_estimate
```python
async def update_estimate(
    estimate_id: str,
    payload: TreatmentEstimateIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- router.post("/{estimate_id}/present") — present_estimate
```python
async def present_estimate(estimate_id: str, request: Request, user: dict = Depends(get_current_user)):
```

- router.post("/{estimate_id}/accept") — accept_estimate
```python
async def accept_estimate(
    estimate_id: str,
    payload: dict,  # contains "accepted_by_name", "patient_signature", "clinic_signature"
    request: Request,
    user: dict = Depends(get_current_user),
):
```

- router.post("/{estimate_id}/reject") — reject_estimate
```python
async def reject_estimate(estimate_id: str, request: Request, user: dict = Depends(get_current_user)):
```

- router.post("/{estimate_id}/cancel") — cancel_estimate
```python
async def cancel_estimate(
    estimate_id: str,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.post("/{estimate_id}/close") — close_treatment
```python
async def close_treatment(
    estimate_id: str,
    payload: TreatmentCloseIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- router.post("/{estimate_id}/transfer-doctor") — transfer_treatment_doctor
```python
async def transfer_treatment_doctor(
    estimate_id: str,
    payload: dict,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- plans_router.get("/{plan_id}") — get_payment_plan
```python
async def get_payment_plan(plan_id: str, request: Request, user: dict = Depends(get_current_user)):
```

- router.post("/{estimate_id}/payment-plan") — create_custom_payment_plan
```python
async def create_custom_payment_plan(
    estimate_id: str,
    payload: PaymentPlanIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- plans_router.put("/{plan_id}") — update_payment_plan
```python
async def update_payment_plan(
    plan_id: str,
    payload: PaymentPlanIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- plans_router.get("/{plan_id}/installments") — get_plan_installments
```python
async def get_plan_installments(plan_id: str, request: Request, user: dict = Depends(get_current_user)):
```

- router.get("/patient/{patient_id}") — get_patient_estimates
```python
async def get_patient_estimates(patient_id: str, request: Request, user: dict = Depends(get_current_user)):
```

- plans_router.get("/patient/{patient_id}") — get_patient_plans
```python
async def get_patient_plans(patient_id: str, request: Request, user: dict = Depends(get_current_user)):
```

- router.get("/agreements/{plan_id}/print") — get_print_agreement
```python
async def get_print_agreement(plan_id: str, request: Request, user: dict = Depends(get_current_user)):
```

### inventory_router.py

- router.get("") — list_inventory
```python
async def list_inventory(
    request: Request,
    item_type: Optional[str] = None,
    q: Optional[str] = None,
    low_stock: Optional[bool] = None,
    _: dict = Depends(get_current_user),
):
```

- router.post("") — create_item
```python
async def create_item(
    payload: InventoryItemIn,
    request: Request,
    user: dict = Depends(get_current_user),
):
```

- router.put("/{item_id}") — update_item
```python
async def update_item(
    item_id: str,
    payload: InventoryItemUpdateIn,
    request: Request,
    _: dict = Depends(get_current_user),
):
```

- router.delete("/{item_id}") — delete_item
```python
async def delete_item(
    item_id: str,
    request: Request,
    _: dict = Depends(get_current_user),
):
```

- router.post("/{item_id}/adjust") — adjust_stock
```python
async def adjust_stock(
    item_id: str,
    payload: StockAdjustmentIn,
    request: Request,
    user: dict = Depends(get_current_user),
):
```

- router.get("/transactions") — list_transactions
```python
async def list_transactions(
    request: Request,
    _: dict = Depends(get_current_user),
):
```

### laboratory_router.py

- router.post("/orders") — create_laboratory_order
```python
async def create_laboratory_order(
    payload: LaboratoryOrderIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
):
```

- router.get("/orders/{order_id}") — get_laboratory_order
```python
async def get_laboratory_order(order_id: str, request: Request, _: dict = Depends(get_current_user)):
```

- router.post("/orders/{order_id}/provider-payments") — pay_laboratory_provider
```python
async def pay_laboratory_provider(
    order_id: str,
    payload: dict,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

### monthly_financials_router.py

- router.get("/manual-movements") — list_manual_movements
```python
async def list_manual_movements(
    request: Request,
    month: str = Query(..., pattern=r"^\d{4}-\d{2}$"),
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.post("/manual-movements") — create_manual_movement
```python
async def create_manual_movement(
    payload: ManualFinancialMovementIn,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.delete("/manual-movements/{movement_id}") — delete_manual_movement
```python
async def delete_manual_movement(
    movement_id: str,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.get("/monthly-history") — monthly_history
```python
async def monthly_history(
    request: Request,
    period_from: str | None = Query(None, pattern=r"^\d{4}-\d{2}$"),
    period_to: str | None = Query(None, pattern=r"^\d{4}-\d{2}$"),
    user: dict = Depends(get_current_user),
):
```

- router.put("/monthly-history/{period_key}") — update_monthly_history
```python
async def update_monthly_history(
    period_key: str,
    payload: MonthlyFinancialClosureUpdate,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

### patients_router.py

- router.get("") — list_patients
```python
async def list_patients(
    request: Request,
    q: str | None = Query(None, description="Buscar por nombre, teléfono o email"),
    visit_filter: str | None = Query(
        None,
        description="Filtro de seguimiento: all | 3m | 6m | 12m | no_visits | pending_followup",
    ),
    page: int | None = Query(
        None,
        ge=1,
        description="Si se envía, activa paginación server-side y devuelve contadores globales",
    ),
    page_size: int = Query(
        25,
        ge=1,
        le=200,
        description="Cantidad de pacientes por página cuando se especifica page",
    ),
    user: dict = Depends(get_current_user),
):
```

- router.get("/{patient_id}") — get_patient
```python
async def get_patient(
    patient_id: str,
    request: Request,
    user: dict = Depends(get_current_user),
):
```

- router.post("") — create_patient
```python
async def create_patient(
    payload: PatientIn,
    request: Request,
    user: dict = Depends(
        require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION, UserRole.DOCTOR)
    ),
):
```

- router.put("/{patient_id}") — update_patient
```python
async def update_patient(
    patient_id: str,
    payload: PatientIn,
    request: Request,
    user: dict = Depends(
        require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION, UserRole.DOCTOR)
    ),
    version: Optional[int] = Query(None, description="Expected version of patient document")
):
```

- router.delete("/{patient_id}") — delete_patient
```python
async def delete_patient(
    patient_id: str,
    request: Request,
    _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.get("/{patient_id}/clinical-alerts") — get_clinical_alerts
```python
async def get_clinical_alerts(
    patient_id: str,
    request: Request,
    user: dict = Depends(get_current_user),
):
```

- router.put("/{patient_id}/clinical-alerts") — update_clinical_alerts
```python
async def update_clinical_alerts(
    patient_id: str,
    payload: ClinicalAlerts,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.DOCTOR)),
    version: int = Query(..., description="Expected patient document version")
):
```

- router.get("/{patient_id}/document-checklist") — get_document_checklist
```python
async def get_document_checklist(
    patient_id: str,
    request: Request,
    user: dict = Depends(get_current_user),
):
```

- router.patch("/{patient_id}/document-checklist") — update_document_checklist
```python
async def update_document_checklist(
    patient_id: str,
    payload: DocumentChecklist,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
    version: int = Query(..., description="Expected patient document version")
):
```

- router.get("/{patient_id}/payment-profile") — get_patient_payment_profile
```python
async def get_patient_payment_profile(
    patient_id: str,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION, UserRole.DOCTOR))
):
```

- router.patch("/{patient_id}/payment-profile/settings") — update_patient_payment_profile_settings
```python
async def update_patient_payment_profile_settings(
    patient_id: str,
    payload: PaymentProfileSettingsUpdate,
    request: Request,
    user: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)),
    version: int = Query(..., description="Expected patient document version")
):
```

- router.get("/{patient_id}/follow-ups") — list_patient_follow_ups
```python
async def list_patient_follow_ups(
    patient_id: str,
    request: Request,
    user: dict = Depends(
        require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION, UserRole.DOCTOR)
    ),
):
```

- router.post("/{patient_id}/follow-ups") — create_patient_follow_up
```python
async def create_patient_follow_up(
    patient_id: str,
    payload: PatientFollowUpIn,
    request: Request,
    user: dict = Depends(
        require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)
    ),
):
```

- router.patch("/{patient_id}/follow-ups/{followup_id}") — update_patient_follow_up
```python
async def update_patient_follow_up(
    patient_id: str,
    followup_id: str,
    payload: PatientFollowUpUpdateIn,
    request: Request,
    user: dict = Depends(
        require_role(UserRole.OWNER, UserRole.ADMIN, UserRole.RECEPTION)
    ),
):
```

### services_router.py

- router.get("") — list_services
```python
async def list_services(request: Request, _: dict = Depends(get_current_user)):
```

- router.post("") — create_service
```python
async def create_service(
    payload: ServiceIn,
    request: Request,
    _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.put("/{service_id}") — update_service
```python
async def update_service(
    service_id: str,
    payload: ServiceIn,
    request: Request,
    _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

- router.delete("/{service_id}") — delete_service
```python
async def delete_service(
    service_id: str,
    request: Request,
    _: dict = Depends(require_role(UserRole.OWNER, UserRole.ADMIN)),
):
```

### users_router.py

- router.get("") — list_users
```python
async def list_users(request: Request, _: dict = Depends(require_role(UserRole.OWNER))):
```

- router.post("") — create_user
```python
async def create_user(
    payload: UserCreateIn,
    request: Request,
    _: dict = Depends(require_role(UserRole.OWNER)),
):
```

- router.put("/{user_id}") — update_user
```python
async def update_user(
    user_id: str,
    payload: UserUpdateIn,
    request: Request,
    _: dict = Depends(require_role(UserRole.OWNER)),
):
```

- router.delete("/{user_id}") — delete_user
```python
async def delete_user(
    user_id: str,
    request: Request,
    actor: dict = Depends(require_role(UserRole.OWNER)),
):
```

- router.post("/{user_id}/password") — change_password
```python
async def change_password(
    user_id: str,
    payload: PasswordChangeIn,
    request: Request,
    actor: dict = Depends(get_current_user),
):
```
