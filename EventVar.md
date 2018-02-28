using System.Collections.Generic;
using UnityEngine;


public enum VarType
{
	BOOL,
	INT,
}

public class EventVar:MonoBehaviour,ISerializationCallbackReceiver
{
	public static Dictionary<string,bool> boolDic = new Dictionary<string, bool>();
	public static Dictionary<string,int>  intDic = new Dictionary<string, int>();


	[SerializeField]
	private List<string> bool_keys = new List<string>();

	[SerializeField]
	private List<bool> bool_values = new List<bool>();

	[SerializeField]
	private List<string> int_keys = new List<string>();

	[SerializeField]
	private List<int> int_values = new List<int>();



	void ISerializationCallbackReceiver.OnBeforeSerialize ()
	{
		saveDicValueToList<string,bool> (boolDic, bool_keys, bool_values);
		saveDicValueToList<string,int> (intDic, int_keys, int_values);
	}

	void saveDicValueToList<KEY,VALUE>(Dictionary<KEY,VALUE> dic,List<KEY> keyList,List<VALUE> valueList)
	{
		keyList.Clear();
		valueList.Clear();
		foreach(var kvp in dic)
		{
			keyList.Add(kvp.Key);
			valueList.Add(kvp.Value);
		}   

	}

	void ISerializationCallbackReceiver.OnAfterDeserialize ()
	{
		copyListValueToDic<string,bool> (boolDic, bool_keys, bool_values);
		copyListValueToDic<string,int> (intDic, int_keys, int_values);
	}

	void copyListValueToDic<KEY,VALUE>(Dictionary<KEY,VALUE> dic,List<KEY> keyList,List<VALUE> valueList)
	{
		int count = Mathf.Min(keyList.Count, valueList.Count);
		for (int i = 0; i < count; ++i)
		{
			if (dic.ContainsKey(keyList[i])) {
				continue;
			}
			dic.Add(keyList[i], valueList[i]);
		}
	}


	public static void setBool(string key,bool value)
	{
		if (boolDic.ContainsKey(key)) {
			Debug.Log ("已经包含了这个键");
			return;
		}
		boolDic[key] = value;
	}

	public static bool getBool(string key)
	{
		return boolDic[key];
	}


	public static  void setInt(string key,int value)
	{
		if (intDic.ContainsKey(key)) {
			Debug.Log ("已经包含了这个键");
			return;
		}
		intDic [key] = value;
	}

	public static int getInt(string key)
	{
		return intDic[key];
	}
}
