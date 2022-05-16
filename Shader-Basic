Shader "CS01/01-Shader-Basic" //Shader的真正名字  可以是路径式的格式
{
    Properties
    {
        // 材质球参数及UI面板
        // https://docs.unity3d.com/cn/current/Manual/SL-Properties.html
        // https://docs.unity3d.com/cn/current/ScriptReference/MaterialPropertyDrawer.html
        // https://zhuanlan.zhihu.com/p/93194054
        // 所有材质属性声明都遵循以下基本格式：
        // [optional: attribute] name("display text in Inspector", type name) = default value
		_MainTex ("Texture", 2D) = "" {}
		_Float("Float", Float) = 0.0
		_Slider("Slider", Range(0.0, 1.0)) = 0.07
		_Vector("Vector", Vector) = (.34, .85, .92, 1) 
    }

    SubShader
    {
        // 标签属性，有两种：一种是SubShader层级，一种在Pass层级 区别在于放置 Tags 代码块的位置
        // https://docs.unity3d.com/cn/current/Manual/SL-VertexProgramInputs.html
        // https://docs.unity3d.com/cn/current/Manual/SL-PassTags.html
        Tags { "RenderType"="Opaque" }

        pass
        {
            // Pass里面的内容Shader代码真正起作用的地方，
            // 一个Pass对应一个真正意义上运行在GPU上的完整着色器(Vertex-Fragment Shader)
            // 一个SubShader里面可以包含多个Pass，每个Pass会被按顺序执行

            CGPROGRAM // Shader代码从这里开始
            #pragma vertex vert // 指定一个名为"vert"的函数为顶点Shader
            #pragma fragment frag // 指定一个名为"frag"函数为片元Shader
            #include "UnityCG.cginc" // 引用Unity内置的文件

            // 自定义数据结构体, CPU向顶点Shader提供的模型数据
            // 也可以用Unity内置常用的顶点结构
            // appdata_base：位置、法线和一个纹理坐标。
            // appdata_tan：位置、切线、法线和一个纹理坐标。
            // appdata_full：位置、切线、法线、四个纹理坐标和颜色。
            // https://docs.unity3d.com/cn/current/Manual/SL-VertexProgramInputs.html
            struct appdata  
            {
                // 冒号后面的是特定语义词，告诉CPU需要哪些类似的数据
                float4 vertex : POSITION; // 模型空间顶点坐标
                half2 texcoord0 : TEXCOORD0; // 第一套UV
                half2 texcoord1 : TEXCOORD1; // 第二套UV
                half2 texcoord2 : TEXCOORD2; // 第二套UV
                half2 texcoord4 : TEXCOORD3;  // 模型最多只能有4套UV

                half4 color : COLOR; // 顶点颜色
                half3 normal : NORMAL; // 顶点法线
                half4 tangent : TANGENT; // 顶点切线(模型导入Unity后自动计算得到)
            };

            // 自定义数据结构体，顶点着色器输出的数据，也是片元着色器输入数据
            struct v2f {
                float4 pos : SV_POSITION; // 输出裁剪空间下的顶点坐标数据，给光栅化使用，必须要写的数据
                float2 uv : TEXCOORD0; //自定义数据体
                // 插值器：输出后会被光栅化进行插值，而后作为输入数据，进入片元Shader
                // 注意跟上方的TEXCOORD的意义是不一样的，上方代表的是UV，这里可以是任意数据。
                // 最多可以写16个：TEXCOORD0 ~ TEXCOORD15。
                float3 normal : TEXCOORD1;
            };
            
            // Shader内的变量声明，如果跟上面Properties模块内的参数同名，就可以产生链接
            // Unity内置变量：https://docs.unity3d.com/cn/current/Manual/SL-UnityShaderVariables.html
            // Unity内置函数：https://docs.unity3d.com/cn/current/Manual/SL-BuiltinFunctions.html
            sampler2D _MainTex;
            float4 _MainTex_ST;

            // 顶点Shader
            v2f vert (appdata_base v)
            {
                v2f o; // 初始化输出数据
                o.pos = UnityObjectToClipPos(v.vertex); // 将对象空间中的点变换到齐次坐标中的摄像机裁剪空间
                o.uv = v.texcoord * _MainTex_ST.xy + _MainTex_ST.zw;
                o.normal = v.normal;
                return o;
            }

            // 片元Shader
            fixed4 frag (v2f i) : SV_Target // SV_Target表示为：片元Shader输出的目标地（渲染目标）
            { 
                half4 col = float4(i.uv, 0.0, 0.0);
                return col; 
            }

            ENDCG // Shader代码从这里结束
        }
    }
}
