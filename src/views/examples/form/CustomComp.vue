<script lang="tsx">
  import { defineComponent, ref, unref } from 'compatible-vue';
  import { Divider } from 'ant-design-vue';

  import { BasicForm, useForm } from '@/components/form/index';
  import { useMessage } from '@/hooks/core/useMessage';
  import { customSchema, setComplexSchema, uploadDataDemo } from './demoData';
  import { TinymceActionType, Tinymce } from '@/components/tinymce/index';
  // import { UploadContainer } from '@/components/file/index';

  export default defineComponent({
    setup() {
      const { createMessage } = useMessage();
      const [register, { validateFieldsAndScroll }] = useForm({
        labelWidth: 100,
        actionColOptions: {
          span: 24,
        },
        schemas: customSchema,
      });
      async function validateForm() {
        const { values, err } = await validateFieldsAndScroll();
        if (err) {
          return;
        }
        createMessage.success(JSON.stringify(values));
      }
      // 集成富文本
      const tinymceElRef = ref<TinymceActionType | null>(null);

      const [registerComplex, { setFieldsValue }] = useForm({
        labelWidth: 100,
        actionColOptions: {
          span: 24,
        },
        schemas: setComplexSchema(),
      });
      function handleSetFieldsValue() {
        setFieldsValue({
          fieldInput: 'setFieldsValue',
          fieldUpload: [uploadDataDemo],
        });
        unref(tinymceElRef)!.setContent('setFieldsValue');
      }
      //       <template slot="uploadImg">
      //   <UploadContainer />
      // </template>
      // 集成上传组件
      return () => (
        <div class="p-4">
          <Divider>功能,点击后如果需要还原在tab右键刷新页面即可</Divider>
          <a-button onClick={validateForm} class="mx-3">
            手动校验表单
          </a-button>

          <Divider>JSON表单组件-自定义组件示例</Divider>
          <BasicForm
            onRegister={register}
            onChange={(val: any) => {
              createMessage.success(JSON.stringify(val));

              console.log('点击查询按钮:校验通过触发', val);
            }}
          ></BasicForm>
          <Divider>JSON表单组件-集成富文本组件</Divider>
          <a-button onClick={handleSetFieldsValue} class="mb-3">
            设置表单值
          </a-button>
          <BasicForm
            onRegister={registerComplex}
            onChange={(val: any) => {
              createMessage.success(JSON.stringify(val));

              console.log('点击查询按钮:校验通过触发', val);
            }}
          >
            <template slot="tinymce">
              <Tinymce ref={tinymceElRef} />
            </template>
          </BasicForm>
        </div>
      );
    },
  });
</script>
