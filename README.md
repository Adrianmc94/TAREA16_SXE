# TAREA16_SXE EXTENDIENDO EL MODELO 
### Este módulo Odoo extiende el módulo base de Contactos para añadir la funcionalidad de gestión de Fidelización de Clientes.


## 1. Estructura del Módulo
El módulo fue creado bajo la siguiente estructura de directorios:

<img width="420" height="400" alt="image" src="https://github.com/user-attachments/assets/23372f89-ad3c-4074-b65a-2803003ed30d" />


## 2. Modificamos la siguiente línea del arhcivo .yml de nuestro docker-compose:
El servicio web debe tener el volumen mapeado para la detección de módulos:

YAML:

 <img width="421" height="82" alt="image" src="https://github.com/user-attachments/assets/f331067f-e5f9-4aa1-979d-75f32fd1f3d5" />


## 3. Contenido de Archivos Clave
partner_loyalty_program/__manifest__.py <br>
Define los metadatos del módulo y establece la dependencia crítica al módulo contacts para poder extender el modelo res.partner:

<img width="757" height="718" alt="image" src="https://github.com/user-attachments/assets/b86cd5a2-50d2-473f-bb4b-310c4ccb6ab1" />

## 3.1 
partner\_loyalty\_program/models/partner\_loyalty.py <br>
Extiende el modelo res.partner y añade un campo de texto simple (partner_code) y un campo de selección calculado (loyalty_level) con la lógica requerida 
from odoo import models, fields, api

class PartnerLoyalty(models.Model):
    _inherit = 'res.partner'

    partner_code = fields.Char(
        string='Código de Socio',
        help='Campo Nuevo: Un campo de texto para el Código de Socio (ej. "VIP-1234").'
    )

    loyalty_level = fields.Selection(
        [('standard', 'Estándar'),
         ('premium', 'Premium'),
         ('gold', 'Gold')],
        string='Nivel de Fidelidad',
        compute='_compute_loyalty_level',
        store=True,
    )

    @api.depends('partner_code')
    def _compute_loyalty_level(self):
        for partner in self:
            code = partner.partner_code
            if code and code.startswith('G'):
                partner.loyalty_level = 'gold'
            elif code:
                partner.loyalty_level = 'premium'
            else:
                partner.loyalty_level = 'standard'



## 3.2 Configuración del xml:
partner\_loyalty\_program/views/partner\_loyalty\_views.xml
<img width="800" height="775" alt="image" src="https://github.com/user-attachments/assets/06954589-cf7d-4bb9-94f7-3e84e5b78a70" />
<img width="787" height="327" alt="image" src="https://github.com/user-attachments/assets/0f6de0da-582f-486b-9e82-47f49a56f0d9" />

## 3.3
Los archivos __init__.py contienen las sentencias de importación estándar para el paquete Python (from . import models y from . import partner_loyalty).


# Comprobación:
Ahora debemos logeanos en nuestra sesión de Odoo como desarolladores, buscar el nombre del módulo y activarlo.
<img width="857" height="612" alt="image" src="https://github.com/user-attachments/assets/a7e449c7-896e-4e60-a1c6-bf3aa46d81a3" />







    
