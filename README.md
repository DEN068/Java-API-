# Java-API-
Java: знакомство и как пользоваться базовым API (семинары)
Задание на дом :
• Подумать над структурой класса Ноутбук для магазина техники - выделить поля и методы. Реализовать в java.
• Создать множество ноутбуков.
• Написать метод, который будет запрашивать у пользователя критерий (или критерии) фильтрации и выведет ноутбуки, отвечающие фильтру. Критерии фильтрации можно хранить в Map. Например:
“Введите цифру, соответствующую необходимому критерию:
1 - ОЗУ
2 - Объем ЖД
3 - Операционная система
4 - Цвет …
• Далее нужно запросить минимальные значения для указанных критериев - сохранить параметры фильтрации можно также в Map.
• Отфильтровать ноутбуки их первоначального множества и вывести проходящие по условиям.


import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Scanner;

public class Laptop {
    private String brand;
    private int ram;
    private int storage;
    private String operatingSystem;
    private String color;
    
    public Laptop(String brand, int ram, int storage, String operatingSystem, String color) {
        this.brand = brand;
        this.ram = ram;
        this.storage = storage;
        this.operatingSystem = operatingSystem;
        this.color = color;
    }
    
    public String getBrand() {
        return brand;
    }
    
    public int getRam() {
        return ram;
    }
    
    public int getStorage() {
        return storage;
    }
    
    public String getOperatingSystem() {
        return operatingSystem;
    }
    
    public String getColor() {
        return color;
    }
    
    public static void main(String[] args) {
        List<Laptop> laptops = new ArrayList<>();
        laptops.add(new Laptop("Dell", 8, 256, "Windows 10", "Silver"));
        laptops.add(new Laptop("HP", 16, 512, "Windows 10", "Black"));
        laptops.add(new Laptop("Lenovo", 4, 128, "Windows 10", "Gray"));
        
        Map<Integer, String> criteria = new HashMap<>();
        criteria.put(1, "RAM");
        criteria.put(2, "Storage");
        criteria.put(3, "Operating System");
        criteria.put(4, "Color");
        
        Map<String, Object> filters = new HashMap<>();
        
        Scanner scanner = new Scanner(System.in);
        
        for (Map.Entry<Integer, String> entry : criteria.entrySet()) {
            System.out.println("Введите минимальное значение для " + entry.getValue() + ":");
            Object value = scanner.next();
            filters.put(entry.getValue(), value);
        }
        
        List<Laptop> filteredLaptops = filterLaptops(laptops, filters);
        
        for (Laptop laptop : filteredLaptops) {
            System.out.println("Бренд: " + laptop.getBrand() + ", ОЗУ: " + laptop.getRam() + "ГБ, ЖД: " + laptop.getStorage() +
                    "ГБ, ОС: " + laptop.getOperatingSystem() + ", Цвет: " + laptop.getColor());
        }
    }
    
    public static List<Laptop> filterLaptops(List<Laptop> laptops, Map<String, Object> filters) {
        List<Laptop> filteredLaptops = new ArrayList<>();
        
        for (Laptop laptop : laptops) {
            boolean isMatch = true;
            
            for (Map.Entry<String, Object> entry : filters.entrySet()) {
                String criterion = entry.getKey();
                Object filterValue = entry.getValue();
                
                switch (criterion) {
                    case "RAM":
                        int ram = laptop.getRam();
                        if (ram < (int) filterValue) {
                            isMatch = false;
                        }
                        break;
                    case "Storage":
                        int storage = laptop.getStorage();
                        if (storage < (int) filterValue) {
                            isMatch = false;
                        }
                        break;
                    case "Operating System":
                        String operatingSystem = laptop.getOperatingSystem();
                        if (!operatingSystem.equals(filterValue)) {
                            isMatch = false;
                        }
                        break;
                    case "Color":
                        String color = laptop.getColor();
                        if (!color.equals(filterValue)) {
                            isMatch = false;
                        }
                        break;
                }
                
                if (!isMatch) {
                    break;
                }
            }
            
            if (isMatch) {
                filteredLaptops.add(laptop);
            }
        }
        
        return filteredLaptops;
    }
}
