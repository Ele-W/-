#### 简单的读

##### demo

###### 实体类

```java
/**
 * 考勤导入实体类接收
 *
 * @author wanghao
 * @date 2022/11/11
 **/
@Data
public class AttendanceExcelUpload {

    @ExcelProperty(value = "上传时间")
    private String uploadDate;

    @ExcelProperty(value = "学生姓名")
    private String studentName;

    @ExcelProperty(value = "学生手机号")
    private String phone;

    @ExcelProperty(value = "上班时间")
    private String workTime;

    @ExcelProperty(value = "签到时间")
    private String signIn;

    @ExcelProperty(value = "签到状态")
    private String inState;

    @ExcelProperty(value = "下班时间")
    private String outTime; //

    @ExcelProperty(value = "签退时间")
    private String signOut; //

    @ExcelProperty(value = "签退状态")
    private String outState; //

    @ExcelProperty(value = "在岗状态")
    private String statess; //

    private Integer state; //


    //更新时间
    @ExcelIgnore
    private Long updateTime;
    //更新时间
    @ExcelIgnore
    private Long createTime;
    //当前行号
    @ExcelIgnore
    private Integer rowIndex;
    @ExcelIgnore
    private Long schoolId;
    @ExcelIgnore
    private Long positionId;
    @ExcelIgnore
    private Long companyId;
    @ExcelIgnore
    private Long studentId; //学生id
}
```

###### 监听器

```java
/**
 * 导入岗位-easyexcel监听
 *
 * @author wanghao
 * @date 2022/8/23 11:48
 **/
@Slf4j
public class AttendanceExcelUploadListener extends AnalysisEventListener<AttendanceExcelUpload> {

    private Long taskId;

    private Long companyId;

    private InternshipAttendanceService attendanceService;

    private List<AttendanceExcelUpload> list = new ArrayList<>();
    
    //五十条传一次
    private static final int BATCH_COUNT = 50;


    AttendanceExcelUpload attendanceExcelUpload = new AttendanceExcelUpload();


    // 构造函数，添加到监听中
    public AttendanceExcelUploadListener(Long companyId, InternshipAttendanceService attendanceService ){
        this.attendanceService = attendanceService;
        this.companyId = companyId;
    }

    /**
     * 这个每一条数据解析都会来调用
     */
    @Override
    public void invoke(AttendanceExcelUpload attendanceExcelUpload, AnalysisContext analysisContext) {
        //获取当前行号
        Integer rowIndex = analysisContext.readRowHolder().getRowIndex();
        log.info("解析到一条数据:{},当前行号：{}", attendanceExcelUpload,rowIndex);
        //存储行号
        attendanceExcelUpload.setRowIndex(rowIndex);
        attendanceExcelUpload.setCompanyId(companyId);

        long l = System.currentTimeMillis();
        attendanceExcelUpload.setCreateTime(l);
        attendanceExcelUpload.setUpdateTime(l);
        list.add(attendanceExcelUpload);
        // 达到BATCH_COUNT了，需要去存储一次数据库
        if (list.size() >= BATCH_COUNT) {
            saveData();
            // 存储完成清理 list
            list.clear();
        }
        log.info("***************************************");
    }

    @Override
    public void doAfterAllAnalysed(AnalysisContext analysisContext) {
        log.info("数据导入完成");
        //最后导入一次
        saveData();
    }

    /**
     * 调用service进行数据处理
     */
    private void saveData() {
        attendanceService.importAttendance(companyId,list);
    }
}
```

###### Controller层实例

```java
/**
 * 导入考勤
 */
@PostMapping("/importAttendance")
public BaseResult importAttendance(@RequestParam Long companyId, @RequestParam MultipartFile file) throws IOException {
    EasyExcel.read(file.getInputStream(),
            AttendanceExcelUpload.class, new AttendanceExcelUploadListener(companyId,attendanceService)).sheet().doRead();
    return BaseResult.success();
}
```

###### service

```java
/**
 * 批量导入考勤
 **/
boolean importAttendance(Long schoolId, List<AttendanceExcelUpload> attendanceExcelUploads);
```

###### impl

```java
/**
     * 批量添加企业信息
     */
    @Transactional
    public boolean importAttendance(Long companyId, List<AttendanceExcelUpload> attendanceExcelUploads){
        // 获取当前系统时间
        long time = System.currentTimeMillis();

        List<AttendanceExcelUpload> add = new ArrayList<>();
        // 批量插入fresh表中的数据
        for (AttendanceExcelUpload attendanceExcelUpload : attendanceExcelUploads) {

            InternshipAttendance internshipAttendance = new InternshipAttendance();

            // 手机号查找对应的正在实习的学生

            InternshipStudent byPhone = internshipStudentMapper.getByPhone(attendanceExcelUpload.getPhone());
            attendanceExcelUpload.setStudentId(byPhone.getId());
            Integer state = 0;

            if ("迟到".equals(attendanceExcelUpload.getStatess())){
                state = 1;
            }else if ("早退".equals(attendanceExcelUpload.getStatess())){
                state = 2;

            }else if ("旷工".equals(attendanceExcelUpload.getStatess())){
                state = 3;

            }else if("请假".equals(attendanceExcelUpload.getStatess())){
                state = 4;
            }
            attendanceExcelUpload.setState(state);

            add.add(attendanceExcelUpload);
//            try{
//
//                // TODO 改为批量插入
//            }
//            catch (Exception e){
//                e.printStackTrace();
//                throw new BusinessException(302,"第"+attendanceExcelUpload.getRowIndex()+"行出现问题,请检查后重试");
//            }
        }
        attendanceMapper.addList(add);

        return true;
    }
```

###### dao



#### 简单的写

##### demo

###### 实体类



```java
@Data
public class StudentDivisionClassToData {
    @ExcelProperty(value = "学生姓名", index = 1)
    private String name;
    @ExcelProperty(value = "学生性别", index = 2)
    private String sex;
    @ExcelProperty(value = "总分", index = 4)
    private String score;
}
```

###### Controller层实例

```java
/**
 * 导出人员去向
 * @param response
 * @param oldDivisionClassStudent
 * @return
 */
@GetMapping("/downloadPersonTo")
public BaseResult downloadPersonTo(HttpServletResponse response, OldDivisionClassStudent oldDivisionClassStudent) {
    oldDivisionClassStudentService.downloadPersonTo(response, oldDivisionClassStudent);
    return BaseResult.success();
}
```

###### service层

```
/**
 * 导出人员去向
 * @param response
 * @param oldDivisionClassStudent
 */
void downloadPersonTo(HttpServletResponse response, OldDivisionClassStudent oldDivisionClassStudent);
```

###### 实现层

```java
/**
 * 导出人员去向
 */
@Override
public void downloadPersonTo(HttpServletResponse response, OldDivisionClassStudent oldDivisionClassStudent) {
    List<StudentDivisionClassToData> studentDivisionClassToData = oldDivisionClassStudentMapper.excelList(oldDivisionClassStudent);
    response.setCharacterEncoding("utf-8");
    // 这里URLEncoder.encode可以防止中文乱码 当然和easyexcel没有关系
    String fileName = null;
    try {
        // 用浏览器或者用postman
        response.setCharacterEncoding("utf-8");
        fileName = URLEncoder.encode("学生分班去向", "UTF-8");
        response.setHeader("Content-disposition", "attachment;filename=" + fileName + ".xlsx");
        EasyExcel.write(response.getOutputStream(), StudentDivisionClassToData.class).sheet("sheet").doWrite(studentDivisionClassToData);
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

###### dao

```java
//导出
List<StudentDivisionClassToData> excelList(OldDivisionClassStudent oldDivisionClassStudent);
```

###### mapper

```xml
<select id="excelList" resultType="com.huayouedu.education.download.domain.StudentDivisionClassToData">
    SELECT
    before_class_name beforeClassName,
    IF(sex = 0,'男','女') sex,
    finally_class_name finallyClassName,
    score,
    name
    FROM
    education_old_division_class_student
    WHERE
    task_id = #{taskId}
    <if test="finallyClassName != null and finallyClassName != ''">
        AND finally_class_name = #{finallyClassName,jdbcType=VARCHAR}
    </if>
    ORDER BY CAST(finally_class_name AS SIGNED INTEGER)
</select>
```