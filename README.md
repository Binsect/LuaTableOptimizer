# LuaTableOptimizer
simple readonly lua table optimizer 

-----------------------------------------------------------------------------------
Optimize readonly lua table memory via reuse values.

    Lua'table ͨ���������洢��Ϸ�������������ݣ�����������кܶ������ظ������ݣ���ô���
���ñ�ռ�ý϶���ڴ棬����Ӱ������ٶȡ�

����:
1����ȡ�ֶ���ʹ����������ֵ��ΪĬ��ֵ������ɾ��Ĭ��ֵ�ֶ�
2��ʹ���˷�ASCII�ַ����ַ����ֶα���Ϊ��Ҫ�������Ի�������ȡ�滻�������ʶ��
3��Ψһ�����е���table������ָ��ͬһ�����ã��Խ�Լ�ڴ�

   Lua'table commonly use to store configuration data for games. it takes a lot of memory
if it contains many fields with same value. this optimization could improve memory usage
and loading speed.

features:
1: remove default value fields ( store them into metatable )
2: auto localization
3: reuse all table values to save memory


require:
the key of the root table must be all string or number as id


-----------------------------------------------------------------------------------
{
	{
		1,
		2,
		3,
		a = "123",
		b = "123"
	},
	{
		1,
		2,
		3,
		a = "123",
		b = "123"
	},
	{
		1,
		2,
		5,
		a = "123",
		b = "123"
	},
	[9] = {
		1,
		2,
		5,
		a = "123",
		b = "123"
	},
	[11] = {
		1,
		2,
		3,
		a = "123",
		b = "123",
		c = {
			{
				1
			},
			{
				1
			},
			{
				2
			},
			{
				2
			}
		},
		d = {
			{
				1,
				a = 1
			},
			{
				2,
				a = 2
			}
		},
		e = {
			{
				1,
				a = 1
			},
			{
				2,
				a = 2
			}
		}
	},
	[100] = {
		1,
		2,
		3,
		a = "tttt",
		b = "123"
	}
}
-----------------------------------------------------------------------------------
local __rt_1 = {
}
local __rt_2 = {
	1,
	2,
	3
}
local __rt_3 = {
	1,
	2,
	5
}
local __rt_4 = {
	{
		1,
		a = 1
	},
	{
		2,
		a = 2
	}
}
local __rt_5 = {
	1
}
local __rt_6 = {
	2
}
local test = 
{
	__rt_2,
	__rt_2,
	__rt_3,
	[9] = __rt_3,
	[11] = {
		1,
		2,
		3,
		c = {
			__rt_5,
			__rt_5,
			__rt_6,
			__rt_6
		},
		d = __rt_4,
		e = __rt_4
	},
	[100] = {
		1,
		2,
		3,
		a = "tttt"
	}
}
local __default_values = {
	a = "123",
	b = "123",
	c = __rt_1,
	d = __rt_1,
	e = __rt_1
}
do
	local base = { __index = __default_values, __newindex = function() error( "Attempt to modify read-only table" ) end }
	for k, v in pairs( nil ) do
		setmetatable( v, base )
	end
	base.__metatable = false
end

return test
-----------------------------------------------------------------------------------

