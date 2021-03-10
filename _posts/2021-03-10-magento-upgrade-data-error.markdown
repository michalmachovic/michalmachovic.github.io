---
layout: post
date:   2021-03-10 13:13:26 +0200
title:  "Magento2 - Upgrade data error when adding new attributes to custom module"
category: magento2
tags: [magento2]
---
When you are going to add new attributes to your custom module in Magento2, to attribute group which name was previously changed through upgrade script, you may end with error, similar to this:

{% highlight javascript %}
QLSTATE[23000]: Integrity constraint violation: 1062 Duplicate entry '4-Images' for key 'EAV_ATTRIBUTE_GROUP_ATTRIBUTE_SET_ID_ATTRIBUTE_GROUP_NAME', query was: INSERT INTO `eav_attribute_group` (`attribute_set_id`, `attribute_group_name`, `sort_order`, `attribute_group_code`) VALUES (?, ?, ?, ?)
{% endhighlight %}

<br /><br />

Here we first rename attribute group with code `my-group` to `NEW NAME OF MY GROUP`. Then we are adding two new attributes to this group, `textual_product_id` and `variant_id`. As you can see, we are not using `group` (as in standard approach at the end of this page) as it was causing error. 


<h3>Setup/UpgradeData.php</h3>
{% highlight php %}
if (version_compare($context->getVersion(), '2.3.2.1') < 0) {
				/** @var EavSetup $eavSetup */
				$eavSetup = $this->eavSetupFactory->create(['setup' => $setup]);				
	
				$groupId = (int)$eavSetup->getAttributeGroupByCode(
					\Magento\Catalog\Model\Product::ENTITY,
					'Default',
					'my-group',
					'attribute_group_id'
				);
				
				$eavSetup->updateAttributeGroup(
					\Magento\Catalog\Model\Product::ENTITY,
					'Default', 
					$groupId,  
					'attribute_group_name', 
					'NEW NAME OF MY GROUP'
				);
			}


			if(version_compare($context->getVersion(), '2.3.2.4') < 0)
			{	
				$eavSetup = $this->eavSetupFactory->create(['setup' => $setup]);

				$eavSetup
					->addAttribute(
						\Magento\Catalog\Model\Product::ENTITY,
						'`textual_product_id`',
						[
							'global' => \Magento\Eav\Model\Entity\Attribute\ScopedAttributeInterface::SCOPE_STORE,
							'input' => 'text',
							'label' => 'Textual Product ID (Textual Orders Only)',
							'required' => false,
							'sort_order' => 18,
							'type' => 'text',
							'used_in_product_listing' => true,
							'visible_on_front' => true
					]);

					$id = $eavSetup->getAttributeId(
						\Magento\Catalog\Model\Product::ENTITY,
						'textual_product_id'
					);
			
					$attributeSetId = $eavSetup->getDefaultAttributeSetId(\Magento\Catalog\Model\Product::ENTITY);
					$eavSetup->addAttributeToGroup(\Magento\Catalog\Model\Product::ENTITY, $attributeSetId, 'my-group', $id, 18);



					$eavSetup
					->addAttribute(
						\Magento\Catalog\Model\Product::ENTITY,
						'variant_id',
						[
							'global' => \Magento\Eav\Model\Entity\Attribute\ScopedAttributeInterface::SCOPE_STORE,
							'input' => 'text',
							'label' => 'Variant ID (Textual Orders Only)',
							'required' => false,
							'sort_order' => 20,
							'type' => 'text',
							'used_in_product_listing' => true,
							'visible_on_front' => true
					]);

					$id = $eavSetup->getAttributeId(
						\Magento\Catalog\Model\Product::ENTITY,
						'variant_id'
					);
			
					$attributeSetId = $eavSetup->getDefaultAttributeSetId(\Magento\Catalog\Model\Product::ENTITY);
					$eavSetup->addAttributeToGroup(\Magento\Catalog\Model\Product::ENTITY, $attributeSetId, 'my-group', $id, 20);
					
			}
{% endhighlight %}



<br /><br /><br />
<h2>Standard approach</h2>
By default we are adding new attribute like this, so we are using `group`:

<h3>Setup/UpgradeData.php</h3>
{% highlight php %}
if(version_compare($context->getVersion(), '2.1.6.0') < 0)
			{
				$eavSetup = $this->eavSetupFactory->create(['setup' => $setup]);
	
				$eavSetup
					->addAttribute(
						\Magento\Catalog\Model\Product::ENTITY,
						'pod_ref',
						[
							'global' => \Magento\Eav\Model\Entity\Attribute\ScopedAttributeInterface::SCOPE_STORE,
							'group' => 'My group',
							'input' => 'text',
							'label' => 'Print On Demand Reference',
							'required' => false,
							'sort_order' => 11,
							'type' => 'text',
							'used_in_product_listing' => true,
							'visible_on_front' => true
						]);
			}
{% endhighlight %}            