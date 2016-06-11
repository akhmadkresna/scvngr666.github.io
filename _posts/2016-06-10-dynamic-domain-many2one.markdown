---
layout: post
title: Dynamic domain on many2one based on checkbox
date: {}
categories: odoo
published: true
---
Hey guys.

This time i will share my exeperience creating dynamic domain on many2one field based on boolean fields. In this tutorial i created dynamic domain for the product sub category(many2one) and the product category(boolean). So product sub category will display the list of content based on multiple boolean field. Interesting right. So lets get started.

## 1. Creating the fields

{% highlight ruby %}
_inherit = "product.template"

is_living_room  = fields.Boolean(string='Living Room')
is_kitchen_stuff= fields.Boolean(string='Kitchen & Dining')
is_bed          = fields.Boolean(string='Bedroom')
is_home_office  = fields.Boolean(string='Home Office')
is_other        = fields.Boolean(string='Other')
#=> boolean fields for checkboxes.
sub_categ_ids_m2o = fields.Many2one ('product.sub.category', string='Select Sub category')
#=> dont forget to create the object for many2one relation in this case 'product.sub.category'.
{% endhighlight %}

## 2. Create the function 

{% highlight ruby %}
@api.onchange('is_living_room', 'is_bed', 'is_kitchen_stuff', 'is_home_office', 'is_other')
def onchange_categ(self):
  selected_categ = []
  res={}
  #=> here we go with the beast. the conditional
  
  if self.is_living_room :
  	selected_categ.append ('is_living_room')
  if self.is_living_room is False:
  	if 'is_living_room' in selected_categ :
  		selected_categ.remove ('is_living_room')
  if self.is_bed :
  	selected_categ.append ('is_bed') 
  if self.is_bed is False:
  	if 'is_bed' in selected_categ :
  		selected_categ.remove ('is_bed')
  if self.is_kitchen_stuff :
  	selected_categ.append ('is_kitchen_stuff') 
  if self.is_kitchen_stuff is False:
  	if 'is_kitchen_stuff' in selected_categ :
  		selected_categ.remove ('is_kitchen_stuff')
  if self.is_home_office :
  	selected_categ.append ('is_home_office') 
  if self.is_home_office is False:
  	if 'is_home_office' in selected_categ :
  		selected_categ.remove ('is_home_office')
  if self.is_other :
  	selected_categ.append ('is_other')
  if self.is_other is False:
  	if 'is_other' in selected_categ :
  		selected_categ.remove ('is_other')
  res.update({
  'domain' : {
  'sub_categ_ids_m2o':[('category','=',list(set(selected_categ)))],

  }
  })        
  return res

{% endhighlight %}




