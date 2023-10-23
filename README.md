
    import java.util.*;

class FamilyTree {
    private Map<String, Set<String>> parentChildMap;
    private Set<String> allMembers;

    public FamilyTree() {
        parentChildMap = new HashMap<>();
        allMembers = new HashSet<>();
    }

    public void addMember(String parent, String child) {
        allMembers.add(parent);
        allMembers.add(child);

        Set<String> children = parentChildMap.getOrDefault(parent, new TreeSet<>());
        children.add(child);
        parentChildMap.put(parent, children);
    }

    public void displayTree() {
        String root = findRoot();
        displayTree(root, "", true);
    }

    private String findRoot() {
        Set<String> potentialRoots = new HashSet<>(allMembers);
        parentChildMap.values().forEach(potentialRoots::removeAll);

        if (potentialRoots.size() == 1) {
            return potentialRoots.iterator().next();
        } else {
            throw new IllegalArgumentException("Invalid family tree. Multiple potential roots found.");
        }
    }

    private void displayTree(String member, String prefix, boolean isTail) {
        System.out.println(prefix + (isTail ? "└── " : "├── ") + member);
        Set<String> children = parentChildMap.get(member);
        if (children != null) {
            List<String> childrenList = new ArrayList<>(children);
            for (int i = 0; i < childrenList.size() - 1; i++) {
                displayTree(childrenList.get(i), prefix + (isTail ? "    " : "│   "), false);
            }
            if (!childrenList.isEmpty()) {
                displayTree(childrenList.get(childrenList.size() - 1), prefix + (isTail ? "    " : "│   "), true);
            }
        }
    }
}

public class Main {
    public static void main(String[] args) {
        FamilyTree familyTree = new FamilyTree();

        familyTree.addMember("Root", "Mammoo & Bifathima");
        familyTree.addMember("Mammoo & Bifathima", "Ashraf & Nusrath");
        familyTree.addMember("Mammoo & Bifathima", "Rafeeq & Shabna");
        familyTree.addMember("Mammoo & Bifathima", "Suleikha");
        familyTree.addMember("Mammoo & Bifathima", "Amina");
        familyTree.addMember("Ashraf & Nusrath", "Adil, Athika, Amr");
        familyTree.addMember("Rafeeq & Shabna", "Maryam, Fathima, Eessa");

        System.out.println("Family Tree:");
        familyTree.displayTree();
    }
}
