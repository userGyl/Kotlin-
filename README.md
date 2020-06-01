# 通过碎片传递的实际示例


###值传递
```kotlin
class WeatherCityFragment : Fragment() {
    private var cityId : String by FragmentArgumentDelegate()

    override fun onActivityCreated(savedInstanceState: Bundle?) {
        super.onActivityCreated(savedInstanceState)
        Toast.makeText(activity, cityId, Toast.LENGTH_SHORT).show()
    }

    companion object {
        fun instance(cityId: String) = WeatherCityFragment().apply {
            this.cityId = cityId
        }
    }
}


###接受示例
```kotlin
class FragmentArgumentDelegate<T : Any> : ReadWriteProperty<Fragment, T> {
  override newInstance{
   //通过委托标记的参数创建实例后 读取...
  }

    override operator fun getValue(thisRef: Fragment, property: KProperty<*>): T {
        val args = thisRef.arguments ?: throw IllegalStateException("Cannot read property ${property.name} if no arguments have been set")
        @Suppress("UNCHECKED_CAST")
        return args.get(property.name) as T? ?: throw IllegalStateException("Property ${property.name} could not be read")
    }

    override operator fun setValue(thisRef: Fragment, property: KProperty<*>, value: T) {
        if (thisRef.arguments == null) {
            thisRef.arguments = Bundle()
        }

        val args = thisRef.arguments!!
        val key = property.name

        when (value) {
            is Boolean -> args.putBoolean(key, value)
            is BooleanArray -> args.putBooleanArray(key, value)
            is String -> args.putString(key, value)
            is Int -> args.putInt(key, value)
            is Short -> args.putShort(key, value)
            is Long -> args.putLong(key, value)
            is Byte -> args.putByte(key, value)
            is ByteArray -> args.putByteArray(key, value)
            is Char -> args.putChar(key, value)
            is CharArray -> args.putCharArray(key, value)
            is CharSequence -> args.putCharSequence(key, value)
            is Float -> args.putFloat(key, value)
            is Bundle -> args.putBundle(key, value)
            is Binder -> BundleCompat.putBinder(args, key, value)
            is android.os.Parcelable -> args.putParcelable(key, value)
            is java.io.Serializable -> args.putSerializable(key, value)
            else -> throw IllegalStateException("Type ${value.javaClass.canonicalName} of property ${property.name} is not supported")
        }
    }
}
