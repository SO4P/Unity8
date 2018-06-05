+ 血条（Health Bar）的预制设计。
    + 分别使用 IMGUI 和 UGUI 实现
        + IMGUI
        ```
        using System.Collections;
        using System.Collections.Generic;
        using UnityEngine;


        public class UI2D : MonoBehaviour
        {

            public GUISkin theSkin;
            public float bloodValue = 0.0f;
            private float tmpValue;
            private Rect rctBloodBar;
            private Rect rctUpButton;
            private Rect rctDownButton;
            private bool onoff;

            // Use this for initialization
            void Start()
            {
                rctBloodBar = new Rect(100, 20, 200, 20);
                rctUpButton = new Rect(50, 20, 40, 20);
                rctDownButton = new Rect(50, 50, 40, 20);
                tmpValue = bloodValue;
            }

            void OnGUI()
            {
                GUI.skin = theSkin;
                if (GUI.Button(rctUpButton, "加血"))
                {
                    tmpValue += 0.1f;
                }
                if (GUI.Button(rctDownButton, "减血"))
                {
                    tmpValue -= 0.1f;
                }
                if (bloodValue > 1.0f)
                {
                    tmpValue = 1.0f;
                    bloodValue = 1.0f;
                }
                if (bloodValue < 0.0f)
                {
                    tmpValue = 0.0f;
                    bloodValue = 0.0f;
                }
                bloodValue = Mathf.Lerp(bloodValue, tmpValue, 0.05f);
                GUI.HorizontalScrollbar(rctBloodBar, 0.0f, bloodValue, 0.0f, 1.0f);
            }
        }

        ```
        + UGUI
        使用canvas中的slider,slider脚本如下：
        ```
        using System.Collections;
        using System.Collections.Generic;
        using UnityEngine;
        using UnityEngine.UI;

        public class UI3D : MonoBehaviour {

            public Slider HPStrip;
            public int Hp = 100;
            // Use this for initialization
            void Start () {
                HPStrip.value = HPStrip.maxValue = Hp;
            }

            // Update is called once per frame
            void Update()
            {
                Hp -= 1;
                HPStrip.value = Hp;    //适当的时候对血条执行操作
            }
        }
        ```
        同时canvas挂载以下脚本保证正对摄像头
        ```
        using System.Collections;
        using System.Collections.Generic;
        using UnityEngine;

        public class CanvasUI : MonoBehaviour {

	        // Use this for initialization
	        void Start () {
		
	        }
	
	        // Update is called once per frame
	        void Update () {
                this.transform.LookAt(Camera.main.transform.position);
            }
        }

        ```
        挂载后会发现血条方向不对，故要将slider先旋转180°，而且我们将canvas的父对象设置为游戏角色，这样可以让血条始终显示在它上面。
    + 分析两种实现的优缺点
        + IMGUI

        优点：实现简单，占用资源少
        
        缺点：位置固定，只能跟着摄像头，效果单一

        + UGUI

        优点：血条可以跟随物体，也能实现正对摄像头等多种效果，可以制作成预制体

        缺点：需要的资源更多
    + 给出预制的使用方法

        IMGUI需将脚本挂载在摄像头上,而UGUI则只需把预制体cube拖到场景中就能使用。
