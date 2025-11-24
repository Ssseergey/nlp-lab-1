# nlp-lab-1

### Критерии:
* Диаграмма графа есть в main.ipynb
* Все модели Pydantic
* Код LangChain в main.ipynb
* Запуск - последняя ячейка main.ipynb

### Установка

python 3.12.7

<code/>python -m venv venv</code>

activate venv

<code/>pip3 install -r requirements.txt </code>
<code/>pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu126 </code>

* Надеяться на то, что cuda уже установлена, хотя в новых версиях pytorch вроде есть встроенный cuda-toolkit.
* Без ускорителя ждать результат можно бесконечно.
* Если вариант - только cpu, то стоит добавить в класс модели квантование после загрузки

#### Запуск
* Поправить промпт на нужный в последней ячейке с кодом main.ipynb
* Нажать run all

### Краткое описание пайплайна

Пользователь вводит текст, в котором ожидается товар и страна, а также страна в которой он хочет найти аналог данного товара.

#### Planner
Нода, которая определяет параметры введенные пользователем

#### DescriptionWriter
Нода, которая пишет описание к товару, которые выбрал пользователь

#### ProductRecommender
Нода, которая рекомендует список похожих товаров в target_стране

### ProductCritic
Нода, которая выбирает из списка наиболее подходящий вариант


### Pydantic модели

class PlannerOutput(BaseModel):
*    item: str
*    country: str
*    target_country: str


class ProductDescriptionWriterOutput(BaseModel):
*    item_description: str


class ProductRecommenderOutput(BaseModel):
*    target_items: List[str]
*    taste_similarity: List[float]
*    use_cases: List[str]
*    package: List[str]


class ProductCriticOutput(BaseModel):
*    target_item: str


class PipelineState(BaseModel):
    human_request: Optional[str] = None
    planner: Optional[PlannerOutput] = None
    product_description_writer: Optional[ProductDescriptionWriterOutput] = None
    product_recommender: Optional[ProductRecommenderOutput] = None
    product_critic: Optional[ProductCriticOutput] = None