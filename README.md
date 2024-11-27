# Unity Excel to JSON
엑셀양식을 json 파일로 변환해주는 유니티 내장 도구 입니다. config.txt 파일을 통해 다양한 옵션을 설정할 수 있으며, Enum 정의도 처리합니다.

# 주요 기능
1. 엑셀 내 필드와 타입을 지정해주고 데이터를 채워 변환을 시도하면 json 파일과 만들어진 양식으로 loader 와 data cs 파일을 자동으로 생성해 줍니다.
2. 변환 해준 json 파일과 cs 파일은 config.txt 파일에서 경로를 변경하여 원하는 곳에 직접 저장 할 수 있도록 합니다.
3. useAssets 사용 여부에 따라 유니티 Assets 폴더 내에서 변환하거나 Assets 폴더가 아닌 다른 폴더에서 변환할 수 있습니다.
4. DataLoader.cs 파일에서는 늘어나는 json 파일에 자동으로 Loader를 생성하여 인스턴스를 생성하고 Dictionay로 관리 할 수 있게 합니다.
5. Load된 json Data는 DataLoader.cs 에서 기본적으로 GetByKey(key), GetByIndex(index), GetAll(), Count() 기능을 제공하여 Data를 쉽게 가져올 수 있습니다.

# 사용시 주의사항
1. cs 파일이 Assets 폴더 안에서 자동으로 생성 되도록 했다면 가급적 자동으로 생성 된 cs 파일은 직접 수정하지 마세요.
2. DataLoader.cs 에서 생성자로 loadFunc 인자를 넘겨줄 때 실제 json 파일의 TextAsset을 반환하는 함수를 넘겨줘야 json 데이터를 캐싱 할 수 있습니다.
3. 엑셀 내 A1 Cell 은 반드시 key, A2 Cell 은 int 로 입력해야 합니다.

# 설치 방법
1. 릴리즈 된 ExcelToJson.unitypackage 를 다운로드 합니다.
2. 프로젝트에 Import 합니다.
3. Assets 폴더 내 Excels 폴더를 대상으로 컨텍스트 메뉴를 열고 Excel to Json 기능을 실행합니다.
4. 그럼 사용 준비가 완료 됩니다.

# 필수 요구 패키지
- Newtonsoft Json (com.unity.nuget.newtonsoft-json)

# 사용 방법
(기본 구성을 바탕으로 설명합니다.)
1. Excels/excel_files 폴더에 변환하고자 하는 Excel 파일을 넣어 둡니다. (Enum 값이 필요하다면 Enum.xlsx 파일도 함께 포함하세요.)
2. 엑셀 내에서는 다음과 같이 필드를 정의합니다.
   - 1번 줄 : header (field name)
   - 2번 줄 : type (field type : int, string, Enum<Type>, float, long, double, bool, List<type>)
   - 3번 줄 : description (field description)
3. 엑셀 내 A1 Cell은 'key' A2 Cell은 int 로 입력하고 나머지 필요한 데이터를 채워 줍니다.
4. Enum 타입의 경우 다음과 같이 정의합니다.
   - A 열 : name (enum name)
   - B 열 ~ @ : value (enum value)
5. DataLoader.cs 를 사용하려면 다음과 같은 예시로 인스턴스를 생성하여 사용합니다.
```cs
public class DBManager : MonoBehaviour
{
  private DataLoader _loader;
  private Func<string, TextAsset> _loadFunc = value => Resources.Load<TextAsset>(value);
     
  void Start()
  {
    _loader = new DataLoader(_loadFunc);
  }
}
```
6. allowMultipleSheets 옵션을 사용하게 되면 json과 cs 에
- json_output/ExcelName_SheetName.json
- loader_output/ExcelName_SheetName.cs
형태로 생성 됩니다.

