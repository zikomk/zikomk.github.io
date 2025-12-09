for odoo v9


## Context

Context is a dictionary carries some information like user ID, user language, Timezone, etc by default. You can pass any kind of data/information in context as per your need either from xml or from python (methods) files and then based on that extra information you can write your process code.

Here, I am going to explain how to use context for relational fields and context to pass when fetching possible values with examples.

Use the context to pass in default values for relational object fields.

?Set value in many2one field’s child field as default by using keyword defualt_<many2one field’s child field name>.
For example, set company in journal_id’s many2one field company_id.
<field name="journal_id" context="{'default_company_id': company_id}"
 

Use a context value in python function by fetching value from xml.
For example, suppose you need to access sale order field value directly into sale order line method through context. For that you can pass the sale order field value in order lines context through xml and access it in a python function as per below code.
Pass value in context

<page string="Order Lines">

<field name="'partner_id'" invisible="1"/>  <!-- sale order field-->

   <field name="line_ids" context="{'partner_id': partner_id}">

       <form string="Sales Order Lines">

          <!-- order lines fields -->

        </form>

    </field>

</field>

</page>

 

Access value in python function

def your_order_line_function(self):

  # to fetch sale order partner from context.

  partner = self.env.context.get('partner_id')

  # rest of the code

  return True

  

Use the context in  _defaults method to set any field value.
 

_defaults = {

       'order_partner_id' : context.get('partner_id', False), #return id or False

   }

 
Use the context to pass default values for normal fields.

For example, Set default value in partner name.
<field name="partner_id" context="{'default_name': ‘Agrolait’}">
 

Use for Context is on buttons to pass parameters to object functions.

 

<button string="Submit" type="object" name="my_func" context="{'param' : 'param_val'}" />
 

Use for Context to set default filter and group by records.

For example, Set default filter to display only leave allocation request and group by all records with employees.
<record id="holidays_allocation_approve" model="ir.actions.act_window">

           <field name="name">Leaves Allocation</field>

           <field name="res_model">hr.holidays</field>

           <field name="context">{'default_type':'add', 'group_by':'employee_id'}</field>

</record>

 




 

## Domain

Domain is a condition which is used to filter your data or for searching. A domain expression is a list of conditions. Each condition is a ('field_name', 'operator', ‘value') tuple.

Where,

field_name : The field name in which you want to set domain.

value : The value is evaluated as a Python expression. It can use literal values, such as numbers, booleans, strings, or lists, and can use fields and identifiers available in the evaluation context.

operator : 

The usual comparison operators are < , > , <= , >= , = , != .

'=like' matches against a pattern, where the underscore symbol, _ , matches any single character, and the percentage symbol, % , matches any sequence of characters.

'like' matches against a '%value%' pattern. The 'ilike' is similar but case insensitive.

The 'not like' and 'not ilike' operators are also available.

'child of' finds the children values in a hierarchical relation, for the models configured to support them.

'in' and 'not in' are used to check for inclusion in a given list, so the value should be a list of values. When it is to be used When it is to be used in "to-many" relation field the ‘in’ operator behaves like a CONTAINS operator.

 

Here, I am going to explain how to use domain for relational fields and filters to apply when displaying existing records for selection with examples.
 

Use the domain to filter relational object fields records with condition.

Suppose you need to display only customers in partner field, so you can set it in domain as per below code.
In xml file

<field name="partner_id" domain="[('customer','=',True)]" />

or

In python file

partner_id = fields.Many2one('sale.order’, string='Customer',domain=[('customer', '=', true)])

 

Suppose you need to display projects for selected client/partner only, if partner is not selected then display all projects. In that case you can use the ternary operator in domain condition as shown in below code.
<field name="project_ids" widget="many2many_tags" domain="[('partner_id','=?',partner_id)]"/>
 

Use parent object field value in relational field using domain.
<field name="tax_id" widget="many2many_tags" domain="{[('company_id','=',parent.company_id)]}"/>
 

Use dynamic domain through onchange function by following code.
@api.onchange('project_ids')

   def _onchange_project_ids(self):

       domain = {}

       partner_list = []

       if not self.project_ids:

           partner_obj = self.env['res.partner'].search([('customer', '=', True)])

           for partner_ids in partner_obj:

               partner_list.append(partner_ids.id)

          # to assign parter_list value in domain

           domain = {'partner_id': [('id', '=', partner_list)]} 

 

       return {'domain': domain}

 

Use the domain display specific records in listview based on condition.

For example to display only those orders which are in ‘sale’ state in sale order list view.
<record id="action_orders" model="ir.actions.act_window">

           <field name="name">Sales Orders</field>

           <field name="type">ir.actions.act_window</field>

           <field name="res_model">sale.order</field>

           <field name="view_type">form</field>

           <field name="view_mode">tree,kanban,form,calendar,pivot,graph</field>

           <field name="search_view_id" ref="view_sales_order_filter"/>

           <field name="domain">[('state', 'in', (‘sale’))]</field>

</record>

 

Use the domain in search view filter.

 

<search string="Product">

      <!-- searches on multiple fields at once -->        

      <field name="name" string="Sales Order" filter_domain="['|',('product_variant_ids.name','ilike',self),('product_variant_ids.attribute_value_ids.name','ilike',self)]"/>

      <filter string="Services" name="services" domain="[('type','=','service')]"/>

</search>

 

Use the domain in fields_view_get() method to set dynamic values for domain filters for a particular field.

For example set department id of the login users.
def fields_view_get(self, cr, uid, view_id=None, view_type='form', context=None, toolbar=False, submenu=False):

   login_user_dpt_id = users_obj.browse(cr, SUPERUSER_ID, uid, context=context).emp_department_id

   for node in eview.xpath("//field[@name='user_id']"):

       if login_user_dpt_id:

           user_filter =  "[('emp_department_id', '='," + str(login_user_dpt_id.id) + " )]"

           node.set('domain',user_filter)

 

Use the domain in record rule.

For example, set domain force to display own payslip which is in draft and done state to that login user.
<record model="ir.rule" id="property_rule_employee_payslip">

      <field name="name">Employee Payslip</field>

      <field name="model_id" ref="model_hr_payslip"/>

      <field name="domain_force">[('employee_id.user_id', '=', user.id),('state','in',['done','draft'])]</field>

      <field name="perm_create" eval="False"/>

      <field name="perm_write" eval="False"/>

      <field name="perm_unlink" eval="False"/>

      <field name="groups"  eval="[(4,ref('base.group_hr_user')),(4,ref('base.group_user'))]"/>

</record>

 

 

Basically domain is a filter you cannot expand beyond - context is a filter you can expand to the point that domain allows.

 